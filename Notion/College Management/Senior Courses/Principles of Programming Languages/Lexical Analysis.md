# Lecture: Deep Dive into Lexical Analysis

Excellent — welcome to a deep, practical, and principled walk through **lexical analysis**. Treat this as the definitive first technical lecture in compiler construction: we’ll cover formal theory, real-world engineering, implementation recipes, pitfalls, performance, security, and practice exercises so you actually *build* a solid lexer.

---

## Overview — what lexical analysis does and why it matters

**Lexical analysis** (a.k.a. *scanning*, *tokenizing*) is the compiler phase that turns a raw stream of characters (source file) into a stream of **tokens** (atomic units such as `IDENT`, `NUMBER`, `+`, `if`, `STRING_LITERAL`, etc.). It:

* Simplifies parsing: parser works on tokens, not characters.
* Filters out irrelevant characters (comments, most whitespace).
* Produces token attributes: lexeme text, literal value, position (line/col), sometimes semantic classes.
* Enforces lexical rules (valid identifiers, numeric literal formats).

Why study it deeply? Because a lexer sits on the hot path for every compile and editor integration (IDE), it must be correct for subtle corner cases (string escapes, Unicode), and it must be robust/fast for large codebases and untrusted input.

---

## 1. Formal foundations (short & essential)

* **Alphabet** `Σ`: finite set of characters (ASCII, UTF-8 code units, or Unicode codepoints).
* **String**: finite sequence of characters over `Σ`.
* **Token language**: set of strings matching a token category; most token languages are **regular languages**.
* **Regular expressions** ↔ **NFA** ↔ **DFA**: the scanner can be implemented by converting regex token specs into DFAs (deterministic finite automata) and running the DFA over input.

Important theorems / facts:

* Union of regular languages is regular — we can lex multiple token types at once by combining DFAs.
* DFAs guarantee linear-time scanning (O(n)), while naive regex engines that backtrack can be *exponential* in the worst case.

**Maximal munch (longest match)**: when multiple tokens match at a position, pick the *longest* match; if equal length, prefer token with higher priority (e.g., keyword vs identifier). This rule is critical and built into most lexer generators.

---

## 2. Token specification — what you write as a language designer

Typical token classes:

* **Keywords**: `if`, `for`, `return` (often recognized after identifier production by lookup)
* **Identifiers**: `[A-Za-z_][A-Za-z0-9_]*` (but watch Unicode)
* **Literals**: integer, float, string, char, raw strings, byte arrays
* **Operators & punctuators**: `+`, `-`, `==`, `->`, `;`, `(`, `)`
* **Comments**: `// ...` or `/* ... */` (may be nested or not)
* **Whitespace**: usually ignored except where significant (e.g., Python indentation)
* **Preprocessor directives**: `#include`, `#define` (often scanned specially)

Design notes:

* Keep token regexes **as simple as possible** (complex semantic checks belong to semantic analysis).
* Define **literal normalization** rules (how you convert `"\n"` into a newline character value).
* Decide whether keywords are lexer-level tokens or identifiers subject to symbol-table lookup — both strategies are used.

---

## 3. Implementation strategies

### A. Generated lexers (recommended for many languages)

* Tools: **Flex**, **lex**, **JFlex**, **ANTLR** (lexer mode), **re2c**, **Ragel**, **Ocamllex**, etc.
* Pros: produce DFAs, fast, handle longest-match, battle-tested.
* Cons: extra build step; integrating hand tweaks sometimes tricky.

### B. Hand-written table-driven DFA

* Convert token regexes → combined DFA (subset construction).
* Encode DFA as transition tables (state × input → next state).
* Fast and controllable; used in high-performance compilers (GCC/LLVM have hand-optimized front-ends).

### C. Hand-written state machine (switch/case)

* Many modern compilers use a hand-coded state machine (a cascade of `switch` on state and input).
* Pros: easy to tune for language quirks; minimal infrastructure.
* Cons: more code to maintain; easy to introduce bugs.

### D. Regex engines (high level) — e.g., `re.finditer()` in Python

* Good for quick prototypes and small DSLs.
* Dangerous for production: many regex engines use backtracking; risk of catastrophic backtracking (ReDoS) with untrusted input.

### E. Scanner combinators / functional lexers

* Popular in functional languages (Haskell `alex`, OCaml `ocamllex` or parser combinators).
* Compositionally nice but can be slower if not optimized.

---

## 4. Practical mechanics (what every lexer must implement well)

### 4.1 Input buffering & performance

* **Two-buffer algorithm** (classic): keep two halves, fill as you go, place a sentinel character at the end to detect EOF without extra checks. Reduces branch mispredictions.
* **Single buffer with pointer + limit**: simpler and fine for many cases.
* For large files or streaming (IDEs), use incremental lexing: only re-lex modified regions.
* Use binary reads, memory-mapped files where available for very large inputs.

### 4.2 Line/column tracking

* Maintain `line` and `column` counters.
* When encountering `\r\n` (CRLF) vs `\n` (LF), unify line counting logic.
* For Unicode, `column` in grapheme clusters? Decide: many tools report column in *code points* or *display width* — document your choice.

### 4.3 Token attributes

A token record usually includes:

```text
type: TokenKind
lexeme: string (raw source text)
value: semantic value (e.g., numeric value, unescaped string)
pos: (filename, line, column)
```

Store large lexemes (string literals) in a *string pool* (interning) to avoid duplicates.

### 4.4 Keyword vs identifier handling

Two approaches:

* Lexer returns `IDENT` tokens with lexeme `"if"`; parser / symbol table checks and reclassifies as `IF` if it’s a keyword.
* Lexer returns `IF` token directly for `"if"` using a keyword table lookup (fast).
  Both are valid; second is common in practice.

### 4.5 Escapes and literal parsing

* Parse escapes (`\n`, `\t`, `\xNN`, `\uNNNN`) in the lexer into `value`.
* Validate correctness (e.g., invalid `\x` sequences).
* Consider whether the lexer should check numeric range or leave it to semantic analysis.

---

## 5. Tricky real-world lexing problems & solutions

### 5.1 Context-sensitive lexing

Some constructs require context:

* **Regex literals vs division operator** (JavaScript): need parser context (after `if ( ... )`, a `/` might start a regex).
* **Here documents** (shell, Perl): lexer must capture until a delimiter line.
* **Template strings** with embedded expressions (JS): lexer must switch states.
  Solution: **lexer states** (start conditions), or coordinated lexer/parser communication (rare).

### 5.2 Indentation-sensitive languages (Python offside rule)

Lexer must emit `INDENT` / `DEDENT` tokens. Typical algorithm:

* Track stack of indentation levels.
* On newline, count spaces/tabs → compare with stack top → emit `INDENT` (push) or multiple `DEDENT` (pop) tokens.
* Handle mixed tabs and spaces carefully — language may define rules.

### 5.3 Nested comments and raw strings

* Some languages allow nested block comments (`(* ... (* ... *) ... *)`). You need a counter (nesting depth) in the lexer.
* Raw strings with arbitrary delimiters — implement delimiter recognition and exact matching.

### 5.4 Unicode and normalization

* Decide whether your lexer operates on UTF-8 bytes, Unicode code points, or grapheme clusters.
* For identifiers: support Unicode letters? Normalize to NFC before comparing keywords/lookup?
* Many languages allow Unicode escapes (e.g., `\u03B1` for α) — convert to proper codepoints.

---

## 6. DFA / regex practical example (small)

Token specs (toy language):

```
WHITESPACE: [ \t\r\n]+
ID: [A-Za-z_][A-Za-z0-9_]*
NUMBER: [0-9]+(\.[0-9]+)?([eE][+-]?[0-9]+)?
PLUS: \+
STAR: \*
LPAREN: \(
RPAREN: \)
ASSIGN: =
COMMENT: //.*    (single-line)
```

**Maximal-munch note**: `"123abc"` should produce `NUMBER(123)` then `ID(abc)`, not `NUMBER(123abc)`.

**Small regex-based Python lexer (prototype)**

```python
import re
TOKEN_SPEC = [
  ('NUMBER',   r'\d+(\.\d+)?([eE][+-]?\d+)?'),
  ('ID',       r'[A-Za-z_]\w*'),
  ('PLUS',     r'\+'),
  ('STAR',     r'\*'),
  ('LPAREN',   r'\('),
  ('RPAREN',   r'\)'),
  ('ASSIGN',   r'='),
  ('COMMENT',  r'//.*'),
  ('WS',       r'[ \t\r\n]+'),
]
master = re.compile('|'.join(f'(?P<{n}>{p})' for n,p in TOKEN_SPEC))

def lex(text):
    for mo in master.finditer(text):
        kind = mo.lastgroup
        lexeme = mo.group(kind)
        if kind == 'WS' or kind == 'COMMENT':
            continue
        yield kind, lexeme

# Example usage
text = "x = 42 + y // add"
print(list(lex(text)))
```

**Caveats**:

* This uses Python’s `re` which might backtrack; safe here but avoid pathological inputs.
* Doesn’t return positions; add `mo.start()` and track lines.
* For production, use generator + incremental buffering.

---

## 7. Hand-written DFA sketch (fast, robust approach)

Pseudocode for table-driven DFA scanner:

```c
state = 0
start = 0
pos = 0
while not EOF:
  state = 0
  last_final_state = -1
  last_final_pos = pos
  start = pos
  while true:
    c = input[pos]
    next = table[state][class_of(c)]
    if next == -1: break
    state = next
    pos += 1
    if final[state]:
        last_final_state = state
        last_final_pos = pos
  if last_final_state == -1:
    error at start
  lexeme = input[start:last_final_pos]
  token = action_for(last_final_state, lexeme)
  emit token
  pos = last_final_pos
```

Key points:

* `class_of(c)` maps many chars to fewer classes (digit, letter, whitespace, etc.) to shrink table.
* `table` is compact (states × classes).
* `final[state]` marks accepting tokens (and priority disambiguation done by table construction).

---

## 8. Error handling & diagnostics

* Detect and report:

  * Unknown characters
  * Unterminated strings/comments
  * Invalid numeric formats
* Provide **clear error messages** with line/column and snippet of source context.
* For IDEs: lexers should be tolerant (produce error tokens but continue) so parser/IDE can still function.

---

## 9. Performance & security considerations

### Performance tips

* Use DFA (non-backtracking) for O(n) worst-case.
* Map input bytes to character classes to reduce table width.
* Inline hot loops, use `switch` on small state if hand-coded.
* Use memory-mapped files or large read buffers.
* Profile & optimize hot paths (string literal scanning often hot).

### Security

* Avoid untrusted input falling into catastrophic backtracking with certain regex patterns (ReDoS). Use linear-time regex engines (e.g., RE2) or DFAs.
* Watch buffer overflows in C implementations.
* Validate numeric literal ranges before using them to prevent integer overflow attacks.

---

## 10. Testing and verification

* Unit tests for each token type.
* Fuzz testing and corpus-driven tests (run lexer on large codebases).
* Property-based tests: round-trip lex → parse → pretty-print should preserve semantics.
* Regression tests for tricky constructs (nested comments, Unicode escapes, indentation behavior).

---

## 11. Special topics (brief)

* **Incremental lexing**: For editors, lex only modified ranges and repair token stream boundaries.
* **Unicode normalization**: For identifier equality, normalize to a canonical form.
* **Token streaming APIs**: Provide peek, lookahead, save/restore for parser needs.
* **Lexer/parser coexistence**: Some tools (ANTLR) tightly integrate lexer and parser and support lexer modes.

---

## 12. Example: Flex file (practical)

```lex
%{
#include "tokens.h"
int lineno = 1;
%}
%%

[ \t]+           ;               /* skip spaces */
\n               { lineno++; }
"/*"([^*]|\*+[^*/])*\*+"/"  ;    /* naive block comment skip */
"//".*           ;               /* single-line comment */

[0-9]+(\.[0-9]+)?([eE][+-]?[0-9]+)?   {
                   yylval.d = atof(yytext);
                   return NUMBER;
                }

[A-Za-z_][A-Za-z0-9_]*    {
                   if (is_keyword(yytext)) return keyword_token(yytext);
                   yylval.s = strdup(yytext);
                   return IDENT;
                }

"=="             return EQ;
"="              return ASSIGN;
"+"              return PLUS;
"-"              return MINUS;
"("              return LPAREN;
")"              return RPAREN;

.                { fprintf(stderr,"Illegal char: %s at %d\n", yytext, lineno); }
%%
```

Notes:

* Flex compiles this to a fast C scanner.
* You still must track `lineno` and produce token attributes.

---

## 13. Exercises (practice makes concrete)

1. **Write a lexer** for a tiny language with identifiers, integers, `+ - * / ( )`, and comments. Produce token stream with positions.
2. **Implement `INDENT`/`DEDENT`** logic like Python’s lexer.
3. **Convert the regexes** for identifiers/numbers to an NFA and show subset construction to produce a DFA for at least one token.
4. **Benchmark:** implement a regex-based and a table-driven lexer on a 10MB codebase; compare throughput and memory.
5. **Fuzz test** your lexer with random bytes and ensure no infinite loops, crashes, or ReDoS behavior.

---

## 14. Summary (cheat-sheet)

* Lexical analysis = characters → tokens.
* Use **DFAs** for linear-time, robust scanning.
* Respect **maximal munch** and token priority.
* Use **lexer states** for context-sensitive constructs.
* Track line/column and produce useful diagnostics.
* Beware regex backtracking; use DFA-based engines or RE2-like libraries in production.
* Test aggressively and consider performance/security early.

---

If you want, I can now:

* Walk you through **building a full lexer** for a small language step-by-step (we can implement a table-driven DFA in Python or C), or
* Show **how to convert regexes → NFA → DFA** with an implemented subset construction and minimization example, or
* Teach **how Python’s INDENT/DEDENT** algorithm works with code.

Which follow-up would you like?
