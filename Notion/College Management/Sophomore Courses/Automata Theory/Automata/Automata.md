  

> [!important] Automata Theory is the study of abstract computing devices, or “machines.”
> 
> - What are the fundamental capabilities and limitations of computers?
> - What can machines compute, and how?

It is the mathematical study of abstract machines (called automata) and the problems they can solve.

An automaton is a simple computational model. It receives input, processes it through a sequence of states, and either accepts or rejects the input.

  

### Alan Turing

In the 1930s, before there were computers, Turing studied an abstract machine that had all the capabilities of today’s computers, at least as far as in what they could compute. **Turing’s goal was to describe precisely the boundary between what a computing machine could do and what it could not do.**

![[/image 11.png|image 11.png]]

He is the Father of the modern computer, an english mathematician.

Turing’s machine is an abstract model of modern-day computer execution and storage

- A mathematical model of computation describing an abstract machine that manipulates symbols on a strip of tape according to a table of rules.

  

/

## Why Study Automata Theory

  

### Introduction to Finite Automata

Finite automata are a useful model for both hardware and software

- Software for designing and checking the behavior of digital circuits
- The lexical analyzer of a typical compiler
- Software for scanning large bodies of text, such as collections of Web pages, to find occurrences of words, phrases or other patterns,
- Software for verifying systems of all types that have a finite number of distinct states

  

There are many systems that may be viewed as being at all times in one of a finite number of **“states.”** The purpose of a state is to remember the relevant portion of the system’s history. Since there are only a finite number of states, the entire history generally cannot be remembered, so the system must be designed carefully, remember what is important and forget what is not.

![[/image 1 8.png|image 1 8.png]]

Simplest nontrivial finite automaton. The device remembers whether it is in the on or the off state, and allows the user to press a button whose effect is different, depending on the state of the switch,

As for all finite automata:

- the states are represented by circles
- Arcs between states are labeled by “inputs”, which represent external influences on the system. The intent of the two arcs is that whichever state the system is in, when the Push input is received it goes to the other state or transitions to another state.
- the start state is the state in which the system is placed initially.
- often there are also final or accepting states. entering one of these states after a sequence of inputs indicates the input sequence is good in some way.

![[/image 2 6.png|image 2 6.png]]

  

# Objectives of Automata

- To develop methods by which computer scientists can describe and analyze the dynamic behavior of discrete systems, in which signals are sampled periodically.

  

  

# Basic Concepts of Automata Theory

  

## Alphabet

- it is a finite, non-empty set of symbols
- Representation:
    - Σ = {0, 1} → binary alphabet
    - Σ = {a, b, c, ..., z} → English lowercase letters
    - Σ = {a, b, #, $} → arbitrary symbols
- These symbols are the building blocks of strings. They’re atomic—they are not broken down further. The alphabet is like a vocabulary or the individual letters that comprise every word.

## Strings

- a string or word is a finite collection of symbols selected from the alphabets Σ
- a string can be empty, which is represented by $\epsilon$ (epsilon).
- The length of a string is denoted by |w| and it is the number of positions for the symbol in the string.
    - If $Σ = \{0,1\}$, then 0101 is a string of length 4.

### Important Terms

|   |   |
|---|---|
|Term|Meaning|
|ε|The **empty string** (length 0)|
||w|
|Σ⁰|Set containing ε|
|Σ¹|All strings of length 1 from Σ|
|Σ*|Set of **all possible strings (incl. ε)**|
|Σ⁺|Set of all **non-empty** strings|

$\epsilon$ is in Σ*, regardless of what alphabet Σ is in. i.e., $\epsilon$ is the only string whose length is 0.

  

### String Operations

|   |   |   |
|---|---|---|
|Operation|Description|Example|
|**Concatenation**|`x • y` = xy|`"ab" • "cd" = "abcd"`|
|**Prefix**|Any leading part|`"ab"` is prefix of `"abc"`|
|**Suffix**|Any trailing part|`"bc"` is suffix of `"abc"`|
|**Substring**|Any part|`"b"` is substring of `"abc"`|
|**Reversal**|`wᴿ`|`"abc"`ᴿ = `"cba"`|
|**Power**|`wⁿ` = w repeated n times|`"ab"² = "abab"`|

  

## Languages

> [!important] A language is just a set of strings over some alphabet.

Example:

If Σ = {0,1}, then:

- L₁ = {`0`, `01`, `011`} is a language.
- L₂ = {w ∈ Σ* | w has even number of 1s}
- L₃ = {w | w is a valid Python identifier}

So, languages = rules + strings

  

  

# Automaton and Its Configuration

  

### Automaton acts as RECOGNIZER or ACCEPTOR

![[/image 3 5.png|image 3 5.png]]

  

### Automaton acts as GENERATOR Or ENUMERATOR or TRANSDUCER

![[/image 4 4.png|image 4 4.png]]

# Finite Automata

- Finite automata are mainly used for pattern recognition
- It takes the string symbol as input and changes its state accordingly
- when the desired symbol is found, then the transition occurs
- at the time of transition, the automata can either move to the next state or stay in the same state
- Finite automata have two states: Accept state or Reject state.

> [!important] It is a mathematical model of a simple computing machine with a finite number of states. It reads a string of symbols and transitions between states based on the current symbol.

It is good for:

- recognizing patterns
- lexical analysis in compilers
- text search engines
- network protocol validation

Any problem where:

- input is processed one symbol at a time
- no need for deep memory or recursion

  

## Two Types of FA

  

### Deterministic Finite Automaton (DFA)

Deterministic Finite Automaton (DFA) is a collection of 5-tuple:

M = (Q, Σ, δ, q₀, F)

Where:

- `Q`: Finite set of **states**
- `Σ`: **Input alphabet** (set of allowed symbols)
- `δ`: **Transition function** `δ: Q × Σ → Q`
- `q₀`: **Start state** (`q₀ ∈ Q`)
- `F`: **Accept states** (`F ⊆ Q`)

✅ **Deterministic**: At any state, given a symbol, there's _exactly one_ transition.

![[/image 5 4.png|image 5 4.png]]

  

![[/image 6 2.png|image 6 2.png]]

  

### Important Points about DFA and NFA

- Every DFA is NFA, but not all NFA is NOT DFA
- There can be multiple final states in both NFA and DFA
- DFA is used in lexical analysis in a compiler
- NFA is more of a theoretical concept.

  

![[/image 7 2.png|image 7 2.png]]

![[/image 8 2.png|image 8 2.png]]

![[/image 9 2.png|image 9 2.png]]

  

![[/image 10 2.png|image 10 2.png]]

![[/image 11 2.png|image 11 2.png]]

![[image 12.png]]