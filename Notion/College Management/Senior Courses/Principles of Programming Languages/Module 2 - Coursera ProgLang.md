This lecture segment provides a crucial conceptual framework for understanding how all data types, in any programming language, are fundamentally constructed. It introduces three universal concepts for building **compound types** and uses this framework to contextualize existing SML types and introduce upcoming topics.

---

# 📚 Standard ML (SML) - Lecture 15: The Three Building Blocks of Types

## 🎯 Learning Objectives

By the end of this lecture, you will be able to:

* Distinguish between **Base Types** and **Compound Types**.
* Identify the three **fundamental ways** to construct any compound type.
* Analyze existing SML types (tuples, options, lists) using this three-part framework.
* Understand where the course is headed with **Records**, **Data Types**, and **Pattern Matching**.

---

## 🏗️ Base Types vs. Compound Types

All types in a programming language can be categorized into two groups:

1.  **Base Types (Atomic Values):** Represent fundamental, indivisible values.
    * **Examples:** `int`, `bool`, `unit`, `char`, `real`.
2.  **Compound Types:** New types built by combining (composing) other types. The pieces inside the compound type are called its **components**.

---

## 🧱 The Three Fundamental Ways to Build Compound Types

Every compound type in any programming language is built using some combination of three core concepts:

### 1. **"Each Of" Types (Product Types)**

* **Concept:** A value of the new type $T$ contains **all** of its components. You must have **this AND this AND that**.
* **Structure:** $T = (\tau_1 \text{ AND } \tau_2 \text{ AND } \dots)$
* **SML Example:** **Tuples** (`t1 * t2 * t3`). A value of type `int * bool` has **both** an `int` and a `bool`.
* **Upcoming SML Feature:** **Records**, which are similar to tuples but use **named fields** instead of positional indices (`#1`, `#2`).

### 2. **"One Of" Types (Sum Types)**

* **Concept:** A value of the new type $T$ contains **one** of its components. You must have **this OR this OR that**.
* **Structure:** $T = (\tau_1 \text{ OR } \tau_2 \text{ OR } \dots)$
* **SML Example:** **Options** (`t option`). An `int option` is either a `NONE` (no data) **OR** a `SOME int`.
* **Upcoming SML Feature:** **Data Types** (Algebraic Data Types), which allow us to define custom "one of" types, e.g., a value is **either** an `int` **or** a `string`.

### 3. **Self-Reference (Recursive Types)**

* **Concept:** The definition of the new type $T$ refers back to $T$ itself. This is essential for defining structures of arbitrary size.
* **Examples:** Lists, Trees, Graphs.

---

## 🔍 Analyzing Existing SML Types

Many common data structures use a combination of these three building blocks:

| SML Type | Construction Components | Structure Breakdown |
| :--- | :--- | :--- |
| **Tuple** (`t1 * t2`) | **Each Of** | A value contains a **$t_1$ AND a $t_2$**. |
| **Option** (`t option`) | **One Of** | A value is **either** a $t$ **OR** it is empty (`NONE`). |
| **List** (`t list`) | **Each Of, One Of, Self-Reference** | An $R$ list is **either** empty (One Of) **OR** it is an **$R$ AND another $R$ list** (Each Of and Self-Reference). |

---

## 🗺️ Path Forward: Implementing the Building Blocks

The next topics in SML will focus on providing flexible language features to define these compound types:

### 1. Custom "Each Of" Types: Records

* **What:** A new way to define "each of" types with **named fields** (e.g., `{x: int, y: int}`) rather than using positional tuples (`int * int`).
* **Syntactic Sugar:** Tuples are so similar to records (just using numeric names) that they can be viewed as "syntactic sugar"—a convenient shorthand—for a more general record concept.

### 2. Custom "One Of" Types: Data Types

* **What:** A way to define custom types that explicitly allow a value to be one of several possibilities (e.g., `type Day = Monday | Tuesday | ...`).

### 3. Accessing the Pieces: Pattern Matching

* **What:** Once we define custom "one of" and "each of" types, we need a powerful, unified way to **access the components** of those new types.
* **Mechanism:** **Pattern Matching** is a powerful feature that allows you to simultaneously check the *form* (or constructor) of a value and extract its internal components in a single, safe operation. It will replace explicit uses of functions like `null`, `hd`, `tl`, `isSome`, and `valOf`.

---

## 💬 Contrast with Object-Oriented Programming (OOP)

OOP (e.g., Java, C++) handles the "one of" concept differently:

* **OOP Method:** Uses **sub-classes** and **sub-typing**. A value is "one of" a superclass or any of its subclasses. Logic is implemented using **virtual methods** (dynamic dispatch).
* **ML Method (Data Types):** Explicitly labels all possibilities at the point of type definition. Logic is implemented using **pattern matching** (static checking).

This fundamental difference is a key takeaway from the course, contrasting the two major paradigms for structuring data and code.

This lecture segment introduces **Records** as a new way to create **"each of" types** in SML, contrasting them with the existing **Tuples**. It emphasizes that records use **named fields**, which is a general design choice in programming languages.

-----

# 📚 Standard ML (SML) - Lecture 16: Records (Named "Each Of" Types)

## 🎯 Learning Objectives

By the end of this lecture, you will be able to:

  * Define and construct **record values** using curly braces and field names.
  * Understand the syntax for **record types** and how SML infers them.
  * Access record components using the **hash symbol and field name** (`#fieldName`).
  * Compare and contrast records with tuples, highlighting the design choice between **access by name** and **access by position**.

-----

## 💾 Part I: Introduction to Records

**Records** are SML's way of creating **"each of" compound types** where the components are accessed by **name** rather than by position (like tuples).

### 1\. Record Construction (Expressions)

Record expressions are enclosed in **curly braces** (`{...}`) and contain a sequence of `fieldName = expression` bindings, separated by commas.

  * **Evaluation:** Each expression is evaluated to a value.
  * **Syntax:**
    ```sml
    val X = {
        bar = 3 + 4,
        foo = true andalso true,
        baz = (false, 9)
    };
    ```

### 2\. Record Values and Order

  * **Value:** A record value is the collection of field names mapped to the resulting values.
      * Example: `val X = {bar = 7, foo = true, baz = (false, 9)}`
  * **Field Order:** The **order of fields does not matter** in the definition, value, or type of a record. The REPL typically prints them in a **canonical order** (alphabetical) for consistency.
      * `{a=1, b=2}` is the same as `{b=2, a=1}`.

### 3\. Record Types

Record types are also enclosed in curly braces and use a colon to map field names to their corresponding types (`fieldName : Type`).

  * **Type Inference:** SML's type checker infers the record type automatically based on the type of each component expression.
  * **Example Type:**
    ```sml
    {
        bar : int,
        baz : bool * int,
        foo : bool
    }
    ```

### 4\. Accessing Record Components

To retrieve a specific component from a record, use the **hash symbol (`#`) followed by the field name**.

  * **Syntax:** `#fieldName recordExpression`
  * **Example:**
    ```sml
    val foo_value = #foo X; (* returns true *)
    val name_value = #name my_niece; (* returns "Amelia" *)
    ```
  * **Note:** Unlike some languages (e.g., C/C++), you do not need to declare a record type before using it; SML infers the type as you define the value.

-----

## 🆚 Part II: Records vs. Tuples

Records and tuples are both methods for defining "each of" types, but they represent a fundamental design choice in programming languages: **access by position vs. access by name.**

| Feature           | Records                                                                                                                    | Tuples                                                                                                  |
| :---------------- | :------------------------------------------------------------------------------------------------------------------------- | :------------------------------------------------------------------------------------------------------ |
| **Access Method** | **By Name** (`#fieldName`)                                                                                                 | **By Position** (`#1`, `#2`, etc.)                                                                      |
| **Purpose/Use**   | Good for structures with **many fields** or where **meaning** is important (e.g., student record, configuration settings). | Good for small collections or generic groupings (e.g., function return values like `(result, status)`). |
| **Syntax**        | `{ field = expression, ... }`                                                                                              | `( expression1, expression2, ... )`                                                                     |
| **Order**         | **Does not matter.** `{a=1, b=2}` is the same as `{b=2, a=1}`.                                                             | **Matters.** `(1, 2)` is **not** the same as `(2, 1)`.                                                  |
| **Type**          | `{ field : Type, ... }`                                                                                                    | `Type1 * Type2 * ...`                                                                                   |

### General Design Choice

The contrast between records and tuples reflects a wider design choice present in many language constructs:

  * **Position-Based Access:** Shorter syntax, but harder to remember what component is at which position (e.g., tuple components, function arguments at the call site).
  * **Name-Based Access:** More verbose, but self-documenting and easier to maintain (e.g., record fields, function arguments at the definition site).

-----

## ➡️ Next Steps: The Tuple-Record Equivalence

Tuples and records are so fundamentally similar that they can be described in terms of each other. In the next segment, we will explore the concept of **syntactic sugar**, demonstrating how tuples are essentially just a convenient shorthand for records using integer field names.

Would you like to examine the idea of **syntactic sugar** and how it relates to tuples and records?


This lecture segment introduces the critical concept of **syntactic sugar** in programming languages, using the relationship between SML's **tuples** and **records** as a concrete example. The key takeaway is that tuples are merely a convenient shorthand for a specific kind of record.

-----

# 📚 Standard ML (SML) - Lecture 17: Tuples as Syntactic Sugar

## 🎯 Learning Objectives

By the end of this lecture, you will be able to:

  * Explain the relationship between SML **tuples** and **records**.
  * Define the term **syntactic sugar** and understand its purpose in language design.
  * Recognize that the core "each of" type constructor in SML is the **record**.

-----

## 🍬 Part I: The Revelation of Tuples

The fundamental revelation in this segment is that **Tuples are not a distinct, primitive data type in SML; they are a shorthand for Records.**

### The SML REPL Demonstration

The proof lies in SML's behavior when you create a record using numeric field names (`1`, `2`, `3`, etc.):

1.  **Record Creation:** If you create a record with consecutive integer field names starting from 1:
    ```sml
    val X = {2 = 4 + 2, 1 = 3 + 1};
    ```
2.  **REPL Output:** The SML Read-Eval-Print Loop (REPL) prints this value and type **exactly as a tuple**:
    ```sml
    val X = (4, 6) : int * int
    ```

### The Rule

Any record that uses field names $1, 2, \dots, N$ (in any order) is treated by SML as an $N$-tuple.

  * **Construction:** `(e1, e2, e3)` is just a shorthand for `{1 = e1, 2 = e2, 3 = e3}`.
  * **Type:** `t1 * t2 * t3` is a shorthand for `{1: t1, 2: t2, 3: t3}`.
  * **Access:** Accessing a component of a tuple, such as `#2 p`, is simply accessing the field named `2` of the underlying record.

**Conclusion:** The true, general **"each of"** type in SML is the **Record**. Tuples exist only to provide a concise, positional syntax for the most common kind of record.

-----

## 🍭 Part II: What is Syntactic Sugar?

**Syntactic Sugar** is an important concept in programming language design that describes a **language construct that adds no new expressive power to the language but makes it "sweeter" (easier, more convenient) to use.**

### 1\. Definition

| Term | Meaning |
| :--- | :--- |
| **Syntactic** | The construct's semantics (evaluation and type-checking rules) can be completely defined by **transforming** its syntax into an equivalent, more fundamental construct that already exists in the language. |
| **Sugar** | It simplifies the user experience by providing a more concise or idiomatic way to write common code patterns. |

### 2\. Benefits of Syntactic Sugar

  * **Simplified Understanding:** Language implementers and advanced users only need to understand the **core** construct (e.g., Records); the "sugared" form (Tuples) is explained via transformation.
  * **Simplified Implementation:** The compiler only has to implement the evaluation and type-checking rules for the core construct (Records). Tuples are simply converted to the record representation before compilation.
  * **Improved Code Style:** It allows programmers to use simple, readable syntax (like `(4, 6)`) instead of verbose, confusing syntax (like `{1=4, 2=6}`).

### 3\. Other Examples of Syntactic Sugar

We have already seen another example of syntactic sugar in SML:

  * The boolean operator **`e1 andalso e2`** is syntactic sugar for the core `if-then-else` expression:
    $$\mathbf{if\ e1\ then\ e2\ else\ false}$$
    While the `if-then-else` version works, `andalso` is cleaner and easier to read.

Syntactic sugar is a pervasive and useful technique for language designers to maintain a small, semantically powerful core while providing a user-friendly surface language.


This lecture segment introduces one of the most powerful and unique features of SML: **Data Types**. This construct allows you to define your own **"one of" types** and introduces **Constructors**, which act as specialized functions and tagged values.

-----

# 📚 Standard ML (SML) - Lecture 18: Custom "One Of" Types (Data Types)

## 🎯 Learning Objectives

By the end of this lecture, you will be able to:

  * Define a custom **data type** in SML using the `datatype` keyword.
  * Understand that a data type creates a **"one of" type**, where a value belongs to one of several distinct possibilities (**variants**).
  * Recognize and use **constructors**—the names for the variants—as both specialized functions and constant values.
  * Compare the access methods of data types to existing "one of" types like Lists and Options.

-----

## 🧱 Part I: Defining Data Types

A **data type** binding is SML's way of creating custom **"one of" types** (also known as **Sum Types**).

### 1\. Syntax and Structure

The `datatype` keyword introduces a new binding that defines a new type name and a set of possibilities (variants), separated by the pipe symbol (`|`).

| Element | Example | Description |
| :--- | :--- | :--- |
| **Keyword** | `datatype` | Starts the binding. |
| **Type Name** | `MyType` | The name of the new type being created. |
| **Constructor** | `TwoInts` | The **tag** or name for a specific variant. **Conventionally capitalized.** |
| **Component Type** | `int * int` | The data carried by that constructor (the "payload"). |
| **Separator** | `|` | Acts as an **"OR"** operator. |

**Example Definition:**

```sml
datatype MyType = TwoInts of int * int 
                | Str of string
                | Pizza;
```

  * **Interpretation:** A value of `MyType` is **either** a `TwoInts` (carrying an `int * int`) **OR** a `Str` (carrying a `string`) **OR** a `Pizza` (carrying nothing).

### 2\. The Power of Data Type Bindings

A single `datatype` binding adds **multiple things** to the environment:

1.  **The New Type Name** (`MyType`).
2.  **The Constructors** (`TwoInts`, `Str`, `Pizza`).

-----

## 🧩 Part II: Constructors and Value Creation

**Constructors** serve two related purposes: they act as **tags** within the value and as specialized **functions** in the environment.

### 1\. Constructors as Functions

Any constructor that carries data (`of Type`) is a function that takes the component type as an argument and returns a value of the new data type.

| Constructor Name | Argument Type | Result Type | Example Usage             |
| :--------------- | :------------ | :---------- | :------------------------ |
| `TwoInts`        | `int * int`   | `MyType`    | `val D = TwoInts (3, 7);` |
| `Str`            | `string`      | `MyType`    | `val A = Str "Hi";`       |

### 2\. Constructors as Values

Any constructor that does **not** carry data is a **constant value** of the new data type.

| Constructor Name | Argument Type | Result Type | Example Usage |
| :--- | :--- | :--- | :--- |
| `Pizza` | `unit` (implicit) | `MyType` | `val C = Pizza;` |

### 3\. Data Type Values (Tag + Data)

A data type value conceptually has two parts:

  * **The Tag Part:** Which constructor (`TwoInts`, `Str`, `Pizza`) was used to create the value.
  * **The Data Part (Payload):** The underlying value(s) carried by the constructor.

**Example Value:** `Str "Hi"`

  * Tag: `Str`
  * Data: `"Hi"`

-----

## 🔑 Part III: The Need for Accessing Data

Whenever we define a new type, we need a way to **access** its components. For "one of" types, this involves two steps:

1.  **Variant Checking (Tag Check):** Determining which constructor (or tag) was used to create the value.
2.  **Data Extraction:** Retrieving the underlying data (if any) carried by that constructor.

### Analogy: Existing "One Of" Types

SML's built-in "one of" types use specialized functions for these tasks:

| Type       | Variant Check (Tag Check)       | Data Extraction |
| :--------- | :------------------------------ | :-------------- |
| **List**   | `null` (is empty?), `not(null)` | `hd`, `tl`      |
| **Option** | `isSome`                        | `valOf`         |

ML *could* have created similar functions for custom data types (e.g., `isStr`, `getStrData`), but this leads to awkward and potentially exception-raising code.

### The Better Way

Instead of defining separate, potentially unsafe access functions for every constructor, SML uses a single, powerful, and safe construct to handle both variant checking and data extraction simultaneously: **Pattern Matching**.

We will explore pattern matching in the next segment, as it is the idiomatic way to safely consume and process data type values.

This lecture segment introduces the **`case` expression** in SML, the primary mechanism used for **pattern matching** against values created from custom **data types**. It is a crucial feature that safely combines variant checking and data extraction.

-----

# 📚 Standard ML (SML) - Lecture 19: Case Expressions and Pattern Matching

## 🎯 Learning Objectives

By the end of this lecture, you will be able to:

  * Use the **`case` expression** to branch on the variants of a data type value.
  * Understand **pattern matching** as the mechanism that safely checks the variant and extracts the data simultaneously.
  * Appreciate the benefits of pattern matching, including **compiler checks for exhaustiveness and redundancy**.

-----

## 🧐 Part I: The Case Expression Syntax

The `case` expression is SML's multi-branch conditional for "one of" types. It takes a value and attempts to match it against a sequence of patterns.

### 1\. Structure

The general syntax involves an expression to evaluate (`e0`) and a list of branches, separated by pipes (`|`).

```sml
case e0 of
    Pattern1 => Expression1
|   Pattern2 => Expression2
|   ...
```

### 2\. Evaluation Rules

1.  **Evaluate $e0$:** The expression between `case` and `of` (the target value) is evaluated first.
2.  **Match Patterns:** The resulting value is compared against `Pattern1`, then `Pattern2`, and so on.
3.  **Take the First Match:** The first pattern that successfully matches the value is chosen.
4.  **Bind Variables:** Any variables within the matching pattern are bound to the corresponding pieces of data extracted from the value.
5.  **Execute Branch:** `ExpressionN` corresponding to the match is evaluated in an environment extended by the new local bindings. The result of the expression becomes the result of the entire `case` expression.

### 3\. Typing Rules

  * All expressions on the right-hand side (`Expression1`, `Expression2`, etc.) **must have the same type**.
  * The type of the entire `case` expression is the common type of its branches.

-----

## 🔀 Part II: Patterns and Data Extraction

A **pattern** is a new kind of expression-like syntax (it is **not** an expression) used for matching and binding variables. For data types, patterns are built using the constructors.

### Example Function: `f` (MyType -\> int)

Using the previously defined type: `datatype MyType = TwoInts of int * int | Str of string | Pizza;`

```sml
fun f(x) =
    case x of
        Pizza => 3                         (* Pattern 1: No data carried. *)
    |   Str s => 8                         (* Pattern 2: Binds variable 's' to the underlying string. *)
    |   TwoInts (i1, i2) => i1 + i2;       (* Pattern 3: Binds i1 and i2 to the components of the pair. *)
```

| Match Case | Pattern Action | Variables in Scope |
| :--- | :--- | :--- |
| Value is `Pizza` | Matches constant tag. | None |
| Value is `Str "Hello"` | Matches `Str` tag and **extracts** `"Hello"`. | `s` is bound to `"Hello"` (type `string`) |
| Value is `TwoInts (7, 9)` | Matches `TwoInts` tag and **extracts** the pair `(7, 9)`. | `i1` is bound to `7` (type `int`), `i2` is bound to `9` (type `int`) |

-----

## 🛡️ Part III: The Safety and Power of Pattern Matching

The primary motivation for using `case` expressions instead of manual variant-checking and extraction functions (like `isStr` and `getStrData`) is **safety and completeness**.

### 1\. Compile-Time Safety Checks

The SML compiler performs two critical checks on every `case` expression:

  * **Exhaustiveness Check (Completeness):** The compiler ensures that **every possible constructor** of the data type has been covered by a pattern.
      * **If a case is missed** (e.g., leaving out `TwoInts`), the compiler issues a **"match nonexhaustive" warning**. If the program reaches an unhandled case at runtime, it results in a fatal `Match` exception.
  * **Redundancy Check:** The compiler ensures that no pattern is placed after a pattern that would make it unreachable.
      * **If a case is unreachable** (e.g., two identical patterns), the compiler issues a **"redundant match" error**, preventing compilation.

### 2\. Guaranteed Safe Extraction

Pattern matching eliminates the need for separate, potentially error-prone extraction functions:

  * When the `Str s` pattern matches, you **know** the value is a `Str` variant, and the data is **safely extracted** into `s`.
  * You never make the mistake of trying to access data that doesn't exist (like applying `hd` to an empty list or `valOf` to `NONE`), because the pattern itself validates the structure.

Pattern matching is far more powerful than this simple introduction suggests. Future segments will explore **richer patterns** that can be used to write even more concise and elegant code.


This lecture segment explores practical and common uses of SML **data types**, focusing on both simple enumeration patterns and more complex, **recursive data structures** that model tree-like data (such as arithmetic expressions). It demonstrates how **recursive functions** and **pattern matching** are used to process these structures.

-----

# 📝 Standard ML (SML) - Lecture 20: Practical Uses of Data Types

## 🎯 Learning Objectives

By the end of this lecture, you will be able to:

  * Use data types to model simple enumerations (enums) and complex alternate identifiers.
  * Understand when to use **"One Of" types (Data Types)** versus **"Each Of" types (Records)**.
  * Define **recursive data types** to model tree-like structures.
  * Write **recursive functions** that operate over recursive data types using pattern matching.

-----

## 🃏 Part I: Modeling Simple "One Of" Scenarios

Data types excel at modeling situations where a value must be **one** of a fixed set of possibilities.

### 1\. Enumerations (Enums)

For sets of distinct, named constants, data types are ideal. Unlike using integers (`1` for club, `2` for diamond), data types enforce correctness and improve readability.

```sml
datatype Suit = Club | Diamond | Heart | Spade;
```

  * **Benefit:** The compiler checks that functions handling `Suit` cover all four possibilities and prevents you from treating a `Suit` value as an integer.

### 2\. Alternate Identifiers (Tags with Data)

Data types are perfect when a single entity can be identified in **mutually exclusive** ways, where only **one** form of data is valid at a time.

```sml
datatype ID = StudentNum of int
            | Name of {first: string, middle: string option, last: string};
```

| Identifier Type | Value | Interpretation |
| :--- | :--- | :--- |
| **StudentNum** | `StudentNum 12345` | This ID **is** a number; the name fields are not relevant. |
| **Name** | `Name {first="Jane", ...}` | This ID **is** a name; the student number is not relevant. |

  * **Avoid Poor Style (Each Of for One Of):** Using a record like `{num: int, name: string}` where you rely on a special value (e.g., `num = -1`) to indicate which field is valid is **poor style**. Data types cleanly enforce the "one of" concept.

### 3\. Combining "Each Of" and "One Of"

To model an entity that is composed of both concepts, use records and options along with data types:

```sml
(* A student has BOTH a name AND an optional number *)
datatype Student = {
    studentNum: int option, (* One Of: Some int OR None *)
    firstName: string,
    middleName: string option,
    lastName: string
};
```

  * **Rule of Thumb:** Use a **Record (Each Of)** when a value *must* contain multiple distinct pieces of information. Use a **Data Type (One Of)** when a value *must* be *one* of several mutually exclusive forms.

-----

## 🌳 Part II: Recursive Data Types (Modeling Trees)

The true power of data types emerges when the type definition refers back to itself, creating **recursive data structures** that model trees. This is fundamental for representing structures like lists, trees, and language grammars.

### Example: Arithmetic Expressions (Trees)

The following data type defines a small language of arithmetic expressions, represented as **Abstract Syntax Trees (ASTs)**.

```sml
datatype X = Const of int        (* Leaf: A simple number *)
           | Neg of X            (* Unary node: Negation of a smaller expression (self-reference) *)
           | Add of X * X        (* Binary node: Addition of two smaller expressions *)
           | Mul of X * X;       (* Binary node: Multiplication of two smaller expressions *)
```

**Example Value (Tree):** `Add(Const (10 + 9), Neg(Const 4))`
This value represents the expression $19 + (-4)$, structured as a tree:

-----

## ♻️ Part III: Recursive Functions Over Data Types

Functions that process recursive data types are almost always **recursive functions** themselves, using **pattern matching** to destructure the tree and process its sub-trees.

### 1\. Function to Evaluate the Expression (`eval`)

This function takes an expression tree (`X`) and computes the resulting integer value.

```sml
fun eval(e) =
    case e of
        Const i       => i           (* Base Case: Return the number *)
    |   Neg e2        => ~(eval e2)  (* Recursive Step: Evaluate subtree, then negate result *)
    |   Add (e1, e2)  => (eval e1) + (eval e2) (* Recursive Step: Evaluate both subtrees, then add results *)
    |   Mul (e1, e2)  => (eval e1) * (eval e2); (* Recursive Step: Evaluate both subtrees, then multiply results *)
```

### 2\. Function to Count Additions (`num_ads`)

This function traverses the tree and counts how many `Add` nodes it contains.

```sml
fun num_ads(e) =
    case e of
        Const _       => 0           (* Base Case: No adds here *)
    |   Neg e2        => num_ads e2  (* Recursive Step: No add at root, count in one subtree *)
    |   Add (e1, e2)  => 1 + (num_ads e1) + (num_ads e2) (* Recursive Step: One add at root, plus adds in both subtrees *)
    |   Mul (e1, e2)  => (num_ads e1) + (num_ads e2); (* Recursive Step: No add at root, count in both subtrees *)
```

**Key Principle:** When processing a recursive data type:

1.  Use a **`case` expression** to handle all constructors.
2.  **Base Cases** handle the leaf constructors (e.g., `Const`).
3.  **Recursive Cases** handle the constructors that contain sub-expressions, making a **recursive call** on each sub-expression and then combining the results.

This lecture segment introduces **Type Synonyms** in SML, contrasting them sharply with **Data Type Bindings** and emphasizing that synonyms merely create an alternative name for an existing type, making them completely interchangeable.

-----

# 📚 Standard ML (SML) - Lecture 21: Type Synonyms

## 🎯 Learning Objectives

By the end of this lecture, you will be able to:

  * Define a **type synonym** using the `type` keyword.
  * Understand that a type synonym is an **alias** for an existing type, not a new type.
  * Recognize that the original type and its synonym are **interchangeable** everywhere in the program.

-----

## 🏷️ Part I: Defining Type Synonyms

A **type synonym** simply gives a new, more descriptive name to a type that is already definable in SML.

### 1\. Syntax

The binding starts with the keyword **`type`** (note: not `datatype`).

```sml
type NewTypeName = ExistingType;
```

### 2\. Distinction from Data Types

| Feature | `datatype` Binding | `type` Synonym |
| :--- | :--- | :--- |
| **Keyword** | `datatype` | `type` |
| **Result** | Introduces a **new, distinct type** that is incompatible with all others. | Introduces a **new name** for an existing type. |
| **Value Creation**| Must use **constructors** (e.g., `Spade`, `TwoInts(...)`). | Values are created just like the `ExistingType` (e.g., as a pair, a record). |
| **Interchangeability**| **No.** A `Suit` is not the same as an `int`. | **Yes.** The new name and the original type are **fully interchangeable**. |

### 3\. Common Idioms

Type synonyms are most useful for making complex types more readable and manageable:

  * **Naming Compound Types (Records/Tuples):** Instead of writing the full type signature everywhere, a short name can be used.
    ```sml
    datatype Suit = Club | Diamond | Heart | Spade;
    datatype Rank = Num of int | Jack | Queen | King | Ace;

    (* Type synonym for a playing card (Suit * Rank pair) *)
    type Card = Suit * Rank;

    (* Type synonym for a complex record *)
    type PersonInfo = { id: int option, name: string };
    ```

-----

## 🔁 Part II: Interchangeability and Equivalence

The core concept of a type synonym is that the new name is **semantically identical** to the original type. They are treated as the same type by the SML compiler and type checker.

  * **Example:** If `type Card = Suit * Rank`, then `Card` and `Suit * Rank` are one and the same.
      * A function expecting a `Card` will happily accept a `Suit * Rank`.
      * A value defined as having type `Suit * Rank` can be given the explicit type signature `Card`.

<!-- end list -->

```sml
(* Function defined using the synonym *)
fun is_Queen_of_Spades(c: Card) = ...

(* The REPL might print the function's type using the original type: *)
(* val is_Queen_of_Spades = fn : Suit * Rank -> bool *)
(* This is OK! The types are equivalent. *)
```

### Consequences for Programming:

  * **Readability:** Using `Card` is much clearer than repeatedly writing `Suit * Rank`.
  * **Flexibility:** When letting SML infer function argument types (e.g., `fun f(c) = ...`), the REPL might arbitrarily choose to print the synonym (`Card`) or the underlying type (`Suit * Rank`). As long as you understand the equivalence, this is not an error.

-----

## 🔮 Part III: Significance of Type Synonyms

### 1\. Convenience (The Immediate Benefit)

The main immediate benefit is simply **convenience and improved documentation**. Synonyms are self-documenting, making complex types easier to read and understand.

### 2\. Module System (The Future Benefit)

While type synonyms don't introduce new capabilities now, they become essential later in the course when studying SML's **Module System**. They will be used to abstract and manage type definitions across different parts of a large program, allowing for advanced software architecture.

Would you like to move on and see how to use **pattern matching** to elegantly access the components of compound types like tuples and records, as an alternative to using `#1`, `#2`, and `#fieldName`?



This lecture segment reveals that SML's built-in **`list`** and **`option`** types are fundamentally defined using **`datatype` bindings** and demonstrates that the correct, idiomatic way to interact with them is using **`case` expressions** and **pattern matching**.

-----

# 📚 SML - Lecture 22: Lists and Options as Data Types

## 🎯 Learning Objectives

By the end of this lecture, you will be able to:

  * Recognize that built-in `list` and `option` types are implemented using **`datatype` bindings**.
  * Use the **constructors** (`NONE`, `SOME`, `[]`, `::` or `colon-colon`) in patterns.
  * Understand why using **`case` expressions** is the preferred and safest style over using helper functions like `isSome`, `valOf`, `hd`, and `tl`.

-----

## 💡 Part I: The Truth About Options and Lists

The built-in `option` and `list` types are not primitive; they are defined using the `datatype` mechanism you just learned.

### 1\. The Option Type

The type $T$ `option` is conceptually defined as:

```sml
datatype 'a option = NONE | SOME of 'a;
```

  * **Constructors:** `NONE` and `SOME` are simply the constructors for this type.

### 2\. The List Type

The type $T$ `list` is conceptually defined as a recursive data structure:

```sml
datatype 'a list = [] | :: of 'a * 'a list;
```

  * **Constructors:**
      * **`[]` (The Empty List):** The constructor for the base case (the end of the list).
      * **`::` (Cons or Colon-Colon):** The constructor for a non-empty list, carrying an element of type `T` and the rest of the list (another `T list`).

-----

## ✍️ Part II: Idiomatic Access via Pattern Matching

Since `option` and `list` are data types, the best way to access their contents is through pattern matching, which provides compiler safety checks.

### 1\. Options: Using `NONE` and `SOME` in Patterns

Instead of using `isSome` and `valOf` (which can raise an exception), use a `case` expression:

```sml
fun option_to_int (x_opt : int option) =
    case x_opt of
        NONE      => 0    (* Pattern 1: Matches the NONE constructor (carries no data) *)
    |   SOME i    => i + 1 (* Pattern 2: Matches the SOME constructor, binds 'i' to the payload *)
```

### 2\. Lists: Using `[]` and `::` in Patterns

Instead of using `null`, `hd`, and `tl` (which can raise exceptions), use `[]` and the infix constructor `::` (or its syntactic counterpart, the colon-colon symbol).

| List Type | Pattern Form | Example Pattern | Meaning |
| :--- | :--- | :--- | :--- |
| **Empty** | `[]` | `[]` | Matches the empty list. |
| **Non-Empty** | Infix `::` (colon-colon) | `x :: xs'` | Binds the head to `x` and the tail to `xs'`. |

**Example: Summing a List**

```sml
fun sum_list(xs) =
    case xs of
        []      => 0                   (* Base Case: Sum of empty list is 0 *)
    |   x :: xs' => x + sum_list(xs')   (* Recursive Step: Add head 'x' to sum of tail 'xs'' *)
```

**Example: Appending Two Lists**

```sml
fun append_list(xs, ys) =
    case xs of
        []      => ys
    |   x :: xs' => x :: (append_list(xs', ys))
```

-----

## ⚖️ Part III: Why Pattern Matching is Superior

Using `case` expressions with these constructors is the **preferred style** in SML for all the reasons discussed previously for custom data types:

1.  **Safety:** It is impossible to make a runtime error like applying `tl` to the empty list or `valOf` to `NONE`.
2.  **Exhaustiveness Check:** The compiler will warn you if you forget to handle the `[]` or `NONE` case, preventing bugs.
3.  **Clarity:** The code structure directly reflects the logic of the algorithm (e.g., "if it's empty, do this; otherwise, take the head and tail...").

> ⚠️ **Homework Requirement:** For subsequent assignments, you will be **required to use `case` expressions and pattern matching** to access the components of lists and options, and **forbidden** from using the helper functions (`null`, `hd`, `tl`, `isSome`, `valOf`).

### Why the Helpers Exist

The built-in helper functions (`hd`, `tl`, `isSome`, etc.) are provided for several reasons:

  * **Convenience:** They are sometimes useful for quick, single-line access (though often less safe).
  * **Higher-Order Functions:** They can be passed as arguments to other functions, which is useful when studying advanced functional programming techniques.
  * **Ease of Definition:** They are easy to define using pattern matching, so the creators included them for consistency.
This lecture segment concludes the discussion on SML's data types by introducing **polymorphic data types**, which allow you to define generic, reusable structures like the built-in `list` and `option`. This demonstrates that those built-in types are not special but are merely pre-defined, polymorphic data types.

-----

# 📚 SML - Lecture 23: Polymorphic Data Types

## 🎯 Learning Objectives

By the end of this lecture, you will be able to:

  * Understand the concept of **polymorphic type constructors** (e.g., `list`, `option`).
  * Define your own **polymorphic data types** using type parameters (e.g., `'a`).
  * Analyze how function code constraints the **polymorphism** of a data type argument (e.g., requiring `'a` to be `int`).

-----

## 🏗️ Part I: Type Constructors vs. Types

Previously, we treated `list` and `option` as special. The key difference is that they are not concrete types; they are **type constructors**.

  * **Type Constructor:** A structure that takes one or more types as parameters to produce a new, concrete type.
      * **Examples:** `list` (takes one type parameter), `option` (takes one type parameter).
  * **Concrete Type:** A type that can be assigned to a value.
      * **Examples:** `int list`, `string option`, `(int list) list`.

A function's type is **polymorphic** (or *generic*) if it works for values of any type, denoted by type variables like `'a` (pronounced "alpha") or `'b` (pronounced "beta").

-----

## ✍️ Part II: Defining Polymorphic Data Types

SML allows you to define your own generic data types using **type parameters**.

### 1\. Syntax for Type Parameters

Type parameters are placed between the `datatype` keyword and the new type name.

```sml
datatype 'a Name = ...
datatype ('a, 'b) Name = ...
```

### 2\. Examples of Polymorphic Data Type Definitions

| Concept | SML Definition | Explanation |
| :--- | :--- | :--- |
| **Option** (Conceptually) | `datatype 'a my_option = NONE | SOME of 'a;` | Takes one type `'a`. `SOME` carries data of that type `'a`. |
| **Linked List** (Custom) | `datatype 'a my_list = EMPTY | CONS of 'a * ('a my_list);` | Takes one type `'a`. The `CONS` constructor holds an element of type `'a'` and recursively holds another `'a' my_list`. |
| **Binary Tree** (Two Types) | `datatype ('a, 'b) tree = LEAF of 'b | NODE of 'a * ('a, 'b) tree * ('a, 'b) tree;` | Takes two type parameters: `'a'` for internal node data and `'b'` for leaf data. |

-----

## 🔎 Part III: Polymorphism in Functions

A function that uses a polymorphic data type will only be as polymorphic as its internal operations allow.

### 1\. Constrained Polymorphism (`sum_tree`)

If a function performs an operation that *requires* a specific type (like arithmetic addition, `+`), the type checker will constrain the generic type parameters.

```sml
fun sum_tree(t) =
    case t of
        LEAF i      => i
    |   NODE (i, l, r) => i + (sum_tree l) + (sum_tree r)
    (* Constraints: Uses addition ('+'), so all data must be 'int' *)
    (* Type: (int, int) tree -> int *)
```

  * `sum_tree` forces both type parameters, `'a'` and `'b'`, to be **`int`** because it attempts to add all data.

### 2\. Partial Polymorphism (`sum_leaves`)

If a function only operates on a subset of the data, the other type parameters can remain polymorphic.

```sml
fun sum_leaves(t) =
    case t of
        LEAF i      => i
    |   NODE (_, l, r) => (sum_leaves l) + (sum_leaves r)
    (* Constraints: Only uses addition on the LEAF data. *)
    (* Type: ('a, int) tree -> int *)
```

  * `sum_leaves` forces the leaf type (`'b'`) to be **`int`**, but the internal node type (`'a'`) remains **polymorphic** because its data is ignored (`_`).

### 3\. Full Polymorphism (`num_leaves`)

If a function only processes the *structure* of the data type and ignores all carried data, it is fully polymorphic.

```sml
fun num_leaves(t) =
    case t of
        LEAF _      => 1
    |   NODE (_, l, r) => (num_leaves l) + (num_leaves r)
    (* Constraints: Uses no data, only counts nodes. *)
    (* Type: ('a, 'b) tree -> int *)
```

  * `num_leaves` is **fully polymorphic**; it works for any type `'a'` and `'b'`.

**The Takeaway:** The power of data types is that they are a unified concept: they can be simple, complex, recursive, and generic. The built-in `list` and `option` types are just common examples of generic data types provided for convenience.



This segment reveals the final truth about function arguments and value bindings in SML: they are all implemented using **pattern matching**, and consequently, **every function in ML takes exactly one argument** (which is often a tuple, treated as **syntactic sugar** for multiple arguments).

-----

# 💡 SML - Lecture 24: The Truth About Functions and Pattern Matching

## 🎯 Learning Objectives

By the end of this lecture, you will be able to:

  * Extend **pattern matching** to "each of" types: **tuples** and **records**.
  * Use patterns in **`val` bindings** and **function arguments** for cleaner code.
  * Understand the "big truth": **Every ML function takes exactly one argument.**

-----

## 🔎 Part I: Pattern Matching for "Each Of" Types

Pattern matching is not just for "one of" types (data types); it is the fundamental mechanism for destructuring **"each of" types** as well.

### 1\. Tuple Patterns

  * **Syntax:** Variables separated by commas inside parentheses.
  * **Mechanism:** Matches a tuple value and binds variables to the components by position.
    ```sml
    (* Pattern for a 3-tuple *)
    val (x, y, z) = (3 + 1, 4 + 2, 5 + 3);
    (* x is bound to 4, y to 6, z to 8 *)
    ```

### 2\. Record Patterns

  * **Syntax:** Field names equals variable names, separated by commas inside braces.
  * **Mechanism:** Matches a record value and binds variables to the component values by field name. **The order doesn't matter.**
    ```sml
    (* Pattern for a record with fields 'first', 'middle', 'last' *)
    val {first=x, middle=y, last=z} = {last="Smith", first="John", middle="A"};
    (* x is bound to "John", y to "A", z to "Smith" *)
    ```

-----

## 📝 Part II: Pattern Matching in Bindings and Arguments

The primary benefit of extending pattern matching is using it directly where bindings are made, leading to the best coding style.

### 1\. The Full Truth About `val` Bindings

The syntax for a `val` binding is actually `val pattern = expression`. The variable name we've always used is just the simplest pattern.

  * **Good Style (`let` expression):** Use patterns in `val` bindings inside a `let` expression to destructure data locally (preferred over single-branch `case` expressions for "each of" types).
    ```sml
    fun sum_triple(t) =
        let
            val (x, y, z) = t (* Pattern matching on the tuple 't' *)
        in
            x + y + z
        end
    ```

### 2\. The Full Truth About Function Bindings (Best Style)

Function arguments themselves can be patterns, leading to the most concise and elegant code for destructuring input.

  * **Best Style (Pattern in Argument):**
    ```sml
    (* Destructures the triple argument directly *)
    fun sum_triple(x, y, z) = x + y + z

    (* Destructures the record argument directly *)
    fun full_name({first=x, middle=y, last=z}) = x ^ " " ^ y ^ " " ^ z
    ```

**Constraint:** After Homework 2, you are **forbidden** from using the old methods (`#1`, `#2`, `#fieldname`) to encourage the use of pattern matching.

-----

## 🤯 Part III: The Big Truth About ML Functions

The appearance of multi-argument functions is actually **syntactic sugar**.

### 1\. The Rule

> **Every function in ML takes exactly one argument.**

### 2\. The Implementation

What we write as a multi-argument function is internally implemented as a function that takes a **single tuple** argument, which is then immediately destructured using **pattern matching**.

| What We Write (Syntactic Sugar) | What ML Sees (The Truth) |
| :--- | :--- |
| `fun f(x, y, z) = ...` | `fun f(t) = let val (x, y, z) = t in ... end` |

### 3\. Benefits of the One-Argument Rule

This policy is elegant and highly flexible, making composition easier:

  * **Piping/Chaining:** Since every function returns a single value (which may be a tuple), the result of one function can be immediately passed as the single argument to the next, regardless of how many logical components that result contains.

    ```sml
    (* rotate_left takes one triple (3,4,5) and returns one triple (4,5,3). *)
    val result = sum_triple (rotate_left (rotate_left (3, 4, 5)));
    (* The inner result is passed seamlessly to the outer call. *)
    ```

  * **Simplified Language:** The language designer only needs to define the semantics for functions that take one argument, reducing complexity.


This lecture segment discusses **type inference** in SML, specifically focusing on how pattern matching enables the type checker to automatically determine argument types and how this can sometimes lead to **unexpected polymorphism** in function signatures.

-----

# 🧠 SML - Lecture 25: Type Inference and Unexpected Polymorphism

## 🎯 Learning Objectives

By the end of this lecture, you will be able to:

  * Understand why **pattern matching** on "each of" types enables full **type inference**.
  * Recognize the limitations of type inference when using the old hash (`#`) character for access.
  * Interpret function types that contain **polymorphic type variables** (e.g., `'a`) when you expected a concrete type.

-----

## 🧐 Part I: Type Inference Advantage of Pattern Matching

SML's **type inference** system can deduce the type of function arguments and variables without explicit type annotations.

### 1\. The Role of Pattern Matching

When you use a pattern in a function argument, you explicitly define the **structure** and the **components** of the argument:

  * **Tuple Pattern (`(x, y, z)`):** The type checker knows the argument must be a **triple**.
  * **Record Pattern (`{first=x, middle=y, last=z}`):** The type checker knows the argument must be a record with those exact **field names**.

The way the variables (`x`, `y`, `z`) are then used in the function body (e.g., adding them, concatenating them) *constrains* the types of their contents (e.g., must be `int`, must be `string`).

| Function | Pattern | Constraint in Body | Inferred Type |
| :--- | :--- | :--- | :--- |
| `sum_triple(x, y, z)` | `(x, y, z)` | `x + y + z` | `int * int * int -> int` |
| `full_name({first=x, ...})`| `{first=x, ...}`| `x ^ ...` | `{first: string, ...} -> string` |

### 2\. The Limitation of Hash Access

Using the old hash character (`#`) for tuple or record access **does not** fully specify the structure:

  * **Tuple Access (`#2 t`):** The type checker knows `t` is a tuple with at least two positions, but **doesn't know how many total positions** it has. Since ML functions cannot take a variable number of arguments (e.g., both 3-tuples and 4-tuples), this ambiguity causes the type checker to **fail** unless you provide an explicit type annotation.
  * **Conclusion:** This is why pattern matching is the required and superior style.

-----

## 😲 Part II: Unexpected Polymorphism

Sometimes, the type checker is "smarter" than you and assigns a function a **more general** type than you expected.

### 1\. Cause

This occurs when a function is **polymorphic** over parts of its input that are not actually used in the computation.

### 2\. Example: `partial_sum`

You write a function that adds the first and third components of a triple:

```sml
fun partial_sum(x, y, z) = x + z
(* The variable 'y' is defined but unused. *)
```

  * **Expected Type:** You might expect `int * int * int -> int`.
  * **Inferred Type:** `int * 'a * int -> int`.

### 3\. Interpretation

The type checker realizes:

  * `x` and `z` must be `int` because they are added (`+`).
  * `y` is **not constrained** by the body's operations. Therefore, the argument corresponding to `y` can be **any type** (`'a`).

This resulting type is **more general** (more flexible) than what you expected.

### 4\. Conclusion for Homework

If ML infers a type that is **more general** (contains type variables) than the concrete type you needed, **it's okay**.

  * **Requirement:** Your function must still work correctly for the specific types required by the assignment (e.g., it must work when `'a'` is an `int`).
  * **Flexibility:** If it works for other types (e.g., `int * string * int`), that is a bonus feature of your code.

The concept of one type being **more general** than another (e.g., `int * 'a * int` is more general than `int * int * int`) has precise rules, which will be covered in the next segment.


This lecture segment clarifies the concept of **type generality** in SML's polymorphic system and introduces the specialized concept of **equality types**, which place constraints on polymorphism based on the built-in equality operator.

-----

# 🎓 SML - Lecture 26: Type Generality and Equality Types

## 🎯 Learning Objectives

By the end of this lecture, you will be able to:

  * Precisely define the rule for one type being **more general** than another.
  * Understand why a more general type satisfies a requirement for a less general type.
  * Recognize and understand the purpose of **equality types** (marked with `''a`).

-----

## ⏫ Part I: Type Generality

Polymorphism allows a function written once to work across many concrete types. A **more general** type encompasses a wider range of possibilities than a less general (or concrete) type.

### 1\. The General Rule

A type $T_1$ is **more general** than a type $T_2$ if and only if:

  * $T_2$ can be produced by **consistently replacing** all type variables (e.g., `'a`, `'b'`) in $T_1$ with concrete types or other type variables.

  * **"Consistently" means:** Every occurrence of a specific type variable (e.g., `'a'`) within $T_1$ must be replaced by the **same** type (e.g., `string`).

### 2\. Examples of Generality

| More General ($T_1$) | Less General ($T_2$) | Consistent? |
| :--- | :--- | :--- |
| `'a` list $\rightarrow$ `'a` list | `int` list $\rightarrow$ `int` list | Yes. (Replaced `'a'` with `int`) |
| `'a` list $\rightarrow$ `'a` list | `string` list $\rightarrow$ `int` list | No. (The three `'a'`s must be the same) |
| `'a * 'a \rightarrow 'a` | `bool * bool \rightarrow bool` | Yes. (Replaced `'a'` with `bool`) |
| `('a * 'b) \rightarrow 'b` | `(int * bool) \rightarrow bool` | Yes. (Replaced `'a'` with `int`, `'b'` with `bool`) |

### 3\. Combining Rules (Synonyms and Records)

When checking generality, you must also apply rules learned previously:

  * **Type Synonyms:** Substitute the synonym's definition. (e.g., `foo` $\equiv$ `int * int`).
  * **Record Fields:** The order of fields does not matter, only the field names and their associated types.

### 4\. Practical Implication

If a homework assignment requires a function to have type $T_2$ (e.g., `string list \rightarrow string list`), and your correct function code results in the type $T_1$ (e.g., `'a` list $\rightarrow$ `'a` list), your solution is acceptable because $T_1$ is more general than $T_2$.

-----

## 🟰 Part II: Equality Types (`''a`)

Equality types are a specialized feature in SML's type system that restricts polymorphism to only those types on which the built-in **equality operator (`=`)** is defined.

### 1\. Syntax

An equality type variable is denoted by two apostrophes: **`''a`**.

### 2\. Meaning

A function with the type $\dots$ **`''a`** $\dots$ is polymorphic, but `'`'a' can only be instantiated with types that are **equality types**.

| Equality Type? | Type | Notes |
| :--- | :--- | :--- |
| **Yes** | `int`, `string`, `bool` | Primitive types are usually equality types. |
| **Yes** | Tuples/Records | Only if **all** of their components are equality types. |
| **No** | `real` (floating-point) | Comparing floating-point numbers with `=` is often a bad practice, so SML forbids it by default. |
| **No** | Function types | Functions cannot be compared for equality. |

### 3\. How Equality Types Arise

Equality types appear when a function uses the **equality operator (`=`)** on its arguments, and the arguments are otherwise unconstrained.

```sml
fun check_equal(x, y) =
    if x = y then "yes" else "no";
(* Inferred Type: ''a * ''a -> string *)
(* The use of '=' forces the arguments to be equality types (''). *)
```

### 4\. Type Coercion by Constraint

If the equality check is performed against a concrete type, the polymorphism disappears:

```sml
fun check_three(x) =
    if x = 3 then "yes" else "no";
(* Inferred Type: int -> string *)
(* Comparing 'x' to '3' (an int) forces 'x' to be an int, eliminating the need for '''a' *)
```



This segment introduces the powerful concept of **nested patterns** in SML, which generalizes pattern matching to allow patterns to appear inside other patterns, enabling more concise and readable code, particularly for operations on complex, structured data like tuples of lists.

-----

# 🧩 SML - Lecture 27: Nested Patterns

## 🎯 Learning Objectives

By the end of this lecture, you will be able to:

  * Understand that patterns can be **nested** (a pattern can appear anywhere a variable could appear in another pattern).
  * Use nested patterns to write concise, single-`case` expressions that destructure complex data structures (like a **triple of lists**).
  * Implement classic functional idioms like **zipping** and **unzipping** using nested patterns.

-----

## 🧐 Part I: The Concept of Nested Patterns

**Nested patterns** allow you to match a value's *shape* at multiple levels of structure simultaneously. This avoids the need for hard-to-read, nested `case` expressions.

  * **Principle:** Everywhere a variable (which is a simple pattern) has been placed inside a larger pattern, you can instead place a more complex pattern (e.g., a tuple pattern, a list pattern, or a constructor pattern).
  * **Mechanism:** Pattern matching becomes a **recursive definition**: the system checks if the value's outer shape matches the outer pattern. If it does, it proceeds to recursively match the inner values against the inner patterns, binding variables only to the parts that match.

-----

## 🔗 Part II: Example 1 - Zipping Lists (`zip3`)

The `zip3` function takes a **triple of lists** and returns a **single list of triples**, combining corresponding elements from the three input lists.

  * **Goal:** Combine three lists: $(L_1, L_2, L_3) \rightarrow [(a_1, b_1, c_1), (a_2, b_2, c_2), \dots]$

<!-- end list -->

```sml
fun zip3(list_triple) =
    case list_triple of
        (* Pattern 1: Match a triple of three EMPTY lists *)
        ([], [], []) => []

    (* Pattern 2: Match a triple of three NON-EMPTY lists *)
    |   (h1::t1, h2::t2, h3::t3) => 
        (h1, h2, h3) :: (zip3 (t1, t2, t3))

    (* Pattern 3: Catch-all for uneven lengths (Error Case) *)
    |   _ => raise UnevenLength
```

### Breakdown of Pattern 2: `(h1::t1, h2::t2, h3::t3)`

1.  **Outer Pattern:** `(P_A, P_B, P_C)` - Requires the input `list_triple` to be a **triple**.
2.  **Inner Patterns (Nested):**
      * `P_A` is `h1::t1` - Requires the first component to be a **non-empty list**, binding the head to `h1` and the tail to `t1`.
      * `P_B` is `h2::t2` - Requires the second component to be a **non-empty list**, binding `h2` and `t2`.
      * `P_C` is `h3::t3` - Requires the third component to be a **non-empty list**, binding `h3` and `t3`.

<!-- end list -->

  * **Result:** This single pattern extracts six variables (`h1` through `t3`) only if all three lists are non-empty, directly simplifying the logic.

-----

## ✂️ Part III: Example 2 - Unzipping Lists (`unzip3`)

The `unzip3` function takes a **list of triples** and returns a **triple of lists**.

  * **Goal:** Separate a list of triples: $[(a_1, b_1, c_1), \dots] \rightarrow (L_A, L_B, L_C)$

<!-- end list -->

```sml
fun unzip3(list) =
    case list of
        (* Pattern 1: Empty list case *)
        [] => ([], [], [])

        (* Pattern 2: Non-empty list case with nested tuple pattern *)
    |   (a, b, c) :: tl => 
        let
            (* Recursively unzip the tail to get the rest of the three lists *)
            val (l1, l2, l3) = unzip3(tl) 
        in
            (* Cons the current head elements (a, b, c) onto the recursively unzipped lists *)
            (a :: l1, b :: l2, c :: l3)
        end
```

### Breakdown of Pattern 2: `(a, b, c) :: tl`

1.  **Outer Pattern:** `P_H :: P_T` - Requires the input `list` to be a **non-empty list**, binding the tail to `tl`.
2.  **Inner Pattern (Nested):** `P_H` is `(a, b, c)` - Requires the head of the list to be a **triple**, binding its components to `a`, `b`, and `c`.

<!-- end list -->

  * **Result:** This single pattern simultaneously checks if the list is non-empty AND if the head element is a triple, cleanly extracting all four necessary variables (`a, b, c, tl`). The code then performs the recursive call and reconstructs the output triple of lists.

This lecture segment introduces **exceptions** in SML as a mechanism for handling runtime errors. It shows how exceptions are defined, raised (thrown), and handled (caught), highlighting the close conceptual similarity between exceptions and data type constructors.

-----

# 🛑 SML - Lecture 28: Exceptions

## 🎯 Learning Objectives

By the end of this lecture, you will be able to:

  * Define new exceptions using the `exception` keyword.
  * Use the `raise` keyword to terminate normal execution and signal an error.
  * Handle (catch) exceptions using the `handle` expression.
  * Understand that exceptions are essentially constructors of the built-in type `exn`.

-----

## 💥 Part I: Defining and Raising Exceptions

Exceptions are used to signal conditions that prevent a function from returning a normal value.

### 1\. Defining Exceptions

Exceptions are defined using the `exception` keyword. This process is analogous to defining a data type constructor.

| Definition | Description | Example Raise |
| :--- | :--- | :--- |
| **Simple Exception** | `exception NAME;` | `raise MyUndesirableCondition;` |
| **Exception with Value** | `exception NAME of TYPE;` | `raise MyOtherException (3, 4);` |

> ⚠️ **Note:** Standard library functions like `hd` use pre-defined exceptions (e.g., `List.Empty`).

### 2\. Raising an Exception

The `raise` keyword stops the current computation and signals the exception to the caller.

```sml
(* The implementation of the list head function *)
fun hd (xs) =
    case xs of
        x::xs' => x
    |   []     => raise List.Empty (* Execution stops here if the list is empty *)

(* Example function raising a custom exception *)
fun my_div(x, y) =
    if y = 0 then
        raise MyUndesirableCondition
    else
        x div y
```

### 3\. Exceptions as Values (Type `exn`)

All exceptions belong to the built-in type **`exn`** (short for exception).

  * You can create an exception *value* without raising it.
  * You can pass `exn` values as arguments to functions.

<!-- end list -->

```sml
fun max_list(xs, exc) =
    case xs of
        []      => raise exc (* Raises the exception value passed in *)
        | [x]     => x
        | x::y::zs => ...
(* Call: max_list([3, 4, 5], MyUndesirableCondition) *)
(* In this case, no exception is raised because the list is not empty. *)
```

-----

## 🛠️ Part II: Handling Exceptions

The `handle` expression allows you to catch an exception raised by an expression and provide an alternative return value.

### 1\. Syntax

The `handle` expression is an extended form of expression:

```sml
E1 handle PATTERN => E2
```

### 2\. Evaluation Rules

1.  **Evaluate $E_1$:**
      * **Success:** If $E_1$ returns a value normally, that value is the result of the entire `handle` expression. $E_2$ is ignored.
      * **Failure (Raise):** If $E_1$ raises an exception, the system checks if the exception value matches the `PATTERN`.
2.  **Match Exception:**
      * **Match:** If the raised exception matches the `PATTERN` (e.g., `MyUndesirableCondition`), then $E_2$ is evaluated. The result of $E_2$ becomes the result of the entire `handle` expression.
      * **No Match:** If the raised exception does *not* match, it is **re-raised** (propagated) outside the `handle` expression.

### 3\. Example Handling

```sml
val x = (max_list([], MyUndesirableCondition)) handle MyUndesirableCondition => 42;

(* Evaluation: *)
(* 1. max_list([], MyUndesirableCondition) is evaluated. *)
(* 2. It raises MyUndesirableCondition. *)
(* 3. The raised exception matches the MyUndesirableCondition pattern. *)
(* 4. The expression '42' is evaluated. *)
(* Result: x is bound to 42. *)
```

> **Advanced:** Like data type bindings, `handle` expressions can use full **pattern matching** on the exception value and can have multiple branches separated by the pipe (`|`) character.

-----

## 🔑 Part III: Summary: Exceptions and Data Types

| Feature | Data Type Constructor | Exception |
| :--- | :--- | :--- |
| **Keyword** | `datatype`, Constructor Name | `exception`, Exception Name |
| **Type** | The declared type (e.g., `Card`) | The built-in type `exn` |
| **Values** | Normal values | Exception values (Type `exn`) |
| **Mechanism** | Used with `case` expressions for normal control flow. | Used with `raise` for error signaling and `handle` for error recovery. |

Exceptions are essentially a reserved set of constructors for the built-in type `exn`, designed specifically for non-local control flow (error handling).

Would you like to review the syntax of `handle` expressions with multiple exception patterns?


This segment introduces **tail recursion**, a crucial concept in functional programming that directly impacts the efficiency of recursive functions by enabling a compiler optimization known as **tail call elimination**.

-----

# 🚀 Tail Recursion and Function Efficiency

## 🎯 Learning Objectives

By the end of this lecture, you will be able to:

  * Understand the concept of the **call stack** and **stack frames** in function execution.
  * Define a **tail call** and a **tail-recursive function**.
  * Explain how **tail call elimination** makes recursion as efficient as iteration (loops).
  * Implement tail-recursive functions using the **accumulator** idiom.

-----

## 💾 Part I: The Call Stack and Stack Frames

To understand the efficiency of recursion, one must first understand how function calls are managed.

  * **Call Stack:** A runtime structure that tracks all active, incomplete function calls.
  * **Stack Frame:** A data structure pushed onto the stack when a function is called. It stores:
      * **Local Variables:** Values bound within the function's scope.
      * **Return Information:** **Crucially, what work is left for the function to do** after any subsequent calls return.

In standard recursion (e.g., the simple `fact(n)`), the stack grows with each recursive call, as the current function needs to perform a final operation (like multiplication) after the recursive result is received.

### Example: Standard Factorial (`fact(n) = n * fact(n-1)`)

| Call | Stack Action | Stack State (Work Left) |
| :--- | :--- | :--- |
| `fact(3)` | Pushes Frame | *Wait for `fact(2)` $\rightarrow$ Multiply result by 3* |
| `fact(2)` | Pushes Frame | *Wait for `fact(1)` $\rightarrow$ Multiply result by 2* |
| `fact(1)` | Pushes Frame | *Wait for `fact(0)` $\rightarrow$ Multiply result by 1* |
| `fact(0)` | Returns 1 | Stack starts to unwind (pop frames) |

$\implies$ **Space Inefficiency:** The stack size is proportional to $N$, which can lead to a **stack overflow** error for very large $N$.

-----

## 🔄 Part II: Tail Recursion and Optimization

### 1\. Tail Call Definition

A function call is a **tail call** if the caller has **no more work to do** after the callee returns.

  * The result of the callee is the result of the caller.
  * The call is the **last operation** performed in the function body.

### 2\. Tail Call Elimination (TCE)

Compilers for functional languages recognize tail calls and perform a key optimization:

  * **Process:** Instead of pushing a new stack frame, the compiler **removes the caller's stack frame** and reuses the same stack space for the callee.
  * **Result:** The stack depth remains constant (usually 1 frame deep) regardless of the depth of recursion.
  * **Efficiency:** A tail-recursive function becomes as efficient as a simple `while` or `for` loop in an imperative language, optimizing both time and space.

### 3\. Tail-Recursive Factorial (`fact_aux(n, acc)`)

The tail-recursive version typically uses a helper function with an **accumulator** (`acc`) argument to pass intermediate results forward.

```sml
fun fact_tail(n) =
    let
        (* acc starts at 1 *)
        fun aux (n, acc) =
            if n = 0 then
                acc (* Result is returned directly *)
            else
                (* The recursive call is the last operation, no multiplication occurs AFTER it returns *)
                aux (n-1, n * acc) 
    in
        aux(n, 1)
    end
```

| Call | Stack Action | Work Left? |
| :--- | :--- | :--- |
| `fact_tail(3)` | Tail call to `aux(3, 1)` | No. |
| `aux(3, 1)` | Tail call to `aux(2, 3)` | No. |
| `aux(2, 3)` | Tail call to `aux(1, 6)` | No. |
| `aux(1, 6)` | Tail call to `aux(0, 6)` | No. |
| `aux(0, 6)` | Returns 6 | Final result. |

$\implies$ **Space Efficiency:** The stack never grows beyond a constant size, making this version suitable for large inputs.



This segment provides a methodology for transforming standard recursive functions into **tail-recursive** ones using the **accumulator** idiom, and demonstrates its application to `sum_list` and, crucially, to `reverse`—showing that tail recursion can also fix non-linear performance issues caused by operators like `append`.

-----

# 💨 Tail Recursion: The Accumulator Idiom

## 🎯 Learning Objectives

By the end of this lecture, you will be able to:

  * Apply the **accumulator idiom** to convert a standard recursive function into a tail-recursive one.
  * Understand the role of the accumulator in replacing the "work left to do" after a recursive call.
  * Analyze how tail recursion (combined with efficient structure use) can improve algorithmic complexity, particularly with the `reverse` function.

-----

## 🏗️ Part I: The Accumulator Methodology

The standard method for achieving tail recursion is by introducing an **auxiliary (helper) function** that carries an extra parameter, the **accumulator** (`acc`).

### 1\. Tail Recursion Definition

A function is **tail-recursive** if all its recursive calls are **tail calls** (the result of the recursive call is immediately returned, with no post-processing work by the caller).

### 2\. The Accumulator Idiom

| Component | Standard Recursive Function | Tail-Recursive Helper Function (`aux`) |
| :--- | :--- | :--- |
| **Initial Call** | Original function calls `aux`, passing the **base case** as the initial accumulator value. |
| **Base Case** | `aux` returns the **final value** held in the accumulator. |
| **Recursive Step** | `aux` calls itself with: 1) Progressed argument (e.g., `n-1` or `t'`); and 2) A **new accumulator** that incorporates the current step's result. |
| **Work** | Accumulator holds the **"answer so far,"** eliminating the need for the caller to do post-recursion work. |

-----

## ➕ Part II: Example - Summing a List (`sum_list`)

The order of addition doesn't matter, making this an ideal candidate for the accumulator pattern.

### 1\. Standard (Non-Tail-Recursive) Version

```sml
fun sum_list_std(x::xs') = x + sum_list_std(xs') (* Work left: add x *)
|   sum_list_std([])  = 0
(* Inefficient: Stack grows with list length. *)
```

### 2\. Tail-Recursive Version

  * **Base Case of Standard:** `0` $\implies$ Initial accumulator is `0`.
  * **Final Result:** The accumulator holds the answer.

<!-- end list -->

```sml
fun sum_list_tail(xs) =
    let
        fun aux([], acc) = acc                 (* Base case: return accumulator *)
        |   aux(x::xs', acc) = aux(xs', x + acc) (* Tail call: new accumulator is updated *)
    in
        aux(xs, 0)
    end
(* Efficient: Stack depth remains constant (O(1)). *)
```

-----

## ↩️ Part III: Example - Reversing a List (`reverse`)

This example highlights that the accumulator idiom can fix poor **algorithmic complexity** caused by costly operators like `append` (`@`).

### 1\. Traditional (Non-Tail-Recursive) Version

```sml
fun reverse_std(x::xs') = (reverse_std(xs')) @ [x] (* Work left: append [x] *)
|   reverse_std([])  = []
```

  * **Complexity Issue:** The `append` operator (`@`) must copy its first list argument.
      * For a list of length $k$, there are $k$ recursive calls.
      * In the $i$-th call, it appends a list of length $i-1$.
      * **Total Work:** $1 + 2 + 3 + \dots + k \approx O(k^2)$. This is quadratically slow.

### 2\. Tail-Recursive Version

  * **Base Case of Standard:** `[]` $\implies$ Initial accumulator is `[]`.
  * **Final Result:** The accumulator holds the reversed list.

<!-- end list -->

```sml
fun reverse_tail(xs) =
    let
        fun aux([], acc) = acc
        |   aux(x::xs', acc) = aux(xs', x :: acc) (* Tail call: use cons (::) to add x to the front *)
    in
        aux(xs, [])
    end
```

  * **Complexity Fix:** This version uses the constant-time **`cons` (`::`)** operator instead of the expensive `append` (`@`).
      * **Total Work:** It performs a constant amount of work at each of the $k$ recursive steps.
      * **Complexity:** $O(k)$. This is linearly fast.

> **Moral:** Beware of recursively using the `append` operator (`@`); it often leads to $O(N^2)$ algorithms when an $O(N)$ solution is possible with an accumulator and the `cons` operator.



This segment concludes the discussion on tail recursion by offering perspective on its importance and providing a **precise, recursive definition** of a **tail position** within an expression.

---

# 📐 Tail Recursion: Perspective and Formal Definition

## 🎯 Learning Objectives

By the end of this lecture, you will be able to:

* Gain perspective on when tail recursion is necessary and when simplicity is preferred.
* Understand the limitations of applying tail recursion (e.g., in tree processing).
* Apply the **recursive definition of tail position** to formally determine if a function call is a tail call.

---

## ⚖️ Part I: Perspective on Tail Recursion

### 1. Limits of Tail Recursion

While tail recursion offers significant efficiency gains (constant stack space), it's not always possible or practical.

* **Tree Processing:** Functions that process tree-like data structures often require the call stack to store **context** (i.e., which branches have been visited and what work remains). It's generally not possible to make all recursive calls tail calls without using other data structures (lists, etc.) that consume just as much auxiliary space.
    * *Example:* You might make the call over the left child tail-recursive, but the need to later process the right child prevents the final step from being a tail call.

* **Simplicity vs. Optimization:** Programmers should not rush to make *every* function tail-recursive.
    * **Priority:** Code should first be **straightforward, readable, and easy to verify** for correctness.
    * **Optimization:** Tail recursion is an **optimization** that should only be applied when performance for large inputs is critical.

---

## 📏 Part II: Formal Definition of Tail Position

The informal definition—"the caller has no more work to do"—can be formalized via a **recursive, top-down definition** of what it means for an expression to be in a **tail position**.

A **tail call** is any function call that occurs in a **tail position**.

### Recursive Rules for Tail Position

The body of a function (`e`) in `fun f p = e` is always in a tail position. For any other expression $E$:

| Expression Type | Tail Position (Subexpressions) | Non-Tail Position (Subexpressions) | Intuition |
| :--- | :--- | :--- | :--- |
| **Conditional** (`if $e_1$ then $e_2$ else $e_3$`) | $e_2$ and $e_3$ | $e_1$ (the test) | Once the condition is tested, the chosen branch is the last thing done. |
| **Case Expression** (`case $e_1$ of $p_2 \implies e_2$ | $\dots$`) | All branch expressions ($e_2$, $e_3$, $\dots$) | $e_1$ (the scrutinizing expression) | The chosen branch result is the final result. |
| **Local Binding** (`let val $b$ in $e$ end`) | $e$ (the body) | All expressions in the bindings $b$ | The body is the last expression evaluated in the scope of the new bindings. |
| **Sequence** (`$e_1$; $e_2$`) | $e_2$ (the last expression) | $e_1$ (the first expression) | The final value comes from $e_2$. |
| **Function Application** (`$e_1$ $e_2$`) | **None** | $e_1$ (function) and $e_2$ (argument) | After evaluating $e_1$ and $e_2$, the function call itself must still be performed (more work). |

**Conclusion:** Once an expression is determined to be in a non-tail position, none of its subexpressions can be in a tail position, as the enclosing expression guarantees more work must occur afterward.

* *Example:* In `f(g(x))`, the call to `g(x)` is in a non-tail position because its result must still be passed to `f`. Only the overall call to `f(g(x))` might be in a tail position.