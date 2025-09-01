
The **syntax** of a programming language is the form of its expressions, statements, and program units

Its **semantics** is the meaning of those expressions, statements and program units.

```
// Example: the while statement in Java

while (<boolean_expr>) <statement>

```
- The semantics of the statement for is that when the current value of the boolean expression is true, embedded statement is executed. 


A **sentence** is a string of characters over some alphabet
A **language** is a set of sentences
A **lexeme** is the lowest level of syntactic unit of a language (e.g., sum, begin)
A **token** is a category of lexemes (e.g, identifier)

```
index = 2*count+17
```


| Lexemes         | Tokens      |
| --------------- | ----------- |
| index and count | identifiers |
| =               | equal_sign  |
| 2 and 17        | int_literal |
| *               | mult_op     |
| +               | plus_op     |
| ;               | semicolon   |


Two ways to formally define languages
- recognition 
- generation


Formal methods of describing syntax
1. Backus-Naur Form and CFG
	- CFG (Noam Chomsky, 1950s)
	- Backus-Naur Form (BNF by John Backus)
		- Metalanguage - a language used to describe another language
		- BNF uses abstractions for syntactic structures


Nonterminal symbols (or simply nonterminals) - the abstractions in BNF descriptions, or grammar
- can have 2 or more distinct definitions
Terminal symbols (or simply terminals) - the lexemes and tokens of the rules

Note:
- A BNF description, or grammar, is simply a collection of rules