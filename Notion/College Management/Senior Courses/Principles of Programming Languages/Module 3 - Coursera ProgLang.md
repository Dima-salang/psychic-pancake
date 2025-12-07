This segment introduces **first-class functions** as a central concept in functional programming, defines related terminology, and shows how functions can be treated as ordinary values in SML.

-----

# 📝 First-Class Functions: Introduction and Concepts

## 🎯 Learning Objectives

By the end of this lecture, you will be able to:

  * Identify the two core ideas of **functional programming**.
  * Define **first-class functions** and recognize how they are used as values.
  * Define and distinguish between **higher-order functions** and **function closures**.

-----

## 🤔 Part I: What is Functional Programming?

Functional programming (FP) is a style of programming built on two core, separate concepts that have historically been combined.

### 1\. Core Concept 1: Avoiding Mutation (Immutability)

  * **Focus:** Not using assignment statements. Data structures are generally **immutable** (cannot change after creation).
  * **Status in ML:** We have studied this extensively by avoiding mutable variables and preferring persistent data structures.

### 2\. Core Concept 2: Functions as Values (First-Class Functions)

  * **Focus:** Treating functions like any other piece of data (values).
  * **Status in ML:** This is the primary focus of the current section.

### Other Functional Programming Characteristics

  * **Heavy Use of Recursion:** Naturally leads to the use of recursive data structures (lists, trees).
  * **Mathematical Style:** Code often resembles mathematical definitions (declarative).
  * **Laziness (Haskell):** A technical term meaning expressions are only evaluated when their result is needed (not a core feature of SML, but will be covered briefly).

> **What is a Functional Language?** A language where functional programming is the **easy, natural, and conventional** way to write code (e.g., SML, Haskell, Racket).

-----

## 💻 Part II: First-Class Functions (Functions as Values)

A function is **first-class** if it can appear and be used anywhere a normal value (like an `int`, `string`, or `list`) can.

### Uses of First-Class Functions

  * **Arguments:** Functions can be passed **as arguments** to other functions.
  * **Results:** Functions can be returned **as results** from other functions.
  * **Bindings:** They can be bound to variables (`val`).
  * **Data Structures:** They can be stored **inside tuples, records, lists,** or data type constructors.

### SML Example

In SML, functions are already first-class. You can place the function name (without arguments) directly into a data structure:

```sml
fun double x = 2 * x;
fun increment x = x + 1;

(* a_tuple is a triple containing two functions and one result *)
val a_tuple = (double, increment, double(increment 7)); 

(* Inferred Type: (int -> int) * (int -> int) * int *)

(* Usage: Extract the double function and call it with 9 *)
val eighteen = #1 a_tuple 9; 
```

-----

## 🏷️ Part III: Terminology

The power of first-class functions is best realized through two related concepts: higher-order functions and closures.

### 1\. Higher-Order Function (HOF)

A function that **takes one or more functions as arguments** OR **returns a function as a result**.

  * **Purpose:** A powerful idiom for **factoring out common computations** and generalizing code (e.g., separating the traversal logic from the operation performed at each step).

### 2\. Function Closure

A function that uses or "closes over" **bindings from outside the function definition** (i.e., variables that are neither arguments nor local variables defined within the function body).

  * **Mechanism:** It captures the environment where it was defined.
  * **Significance:** When functions are passed around (first-class), the way they interact with their original environment becomes more sophisticated and powerful. (This topic will be covered in detail later.)

> **Note on Terminology:** Though conceptually distinct (First-Class = passability; Closure = environment capture), these terms are often used interchangeably because functional languages support both, and the features are frequently used together.



This segment focuses on the most common use of **first-class functions**: passing a function as an argument to another function, creating a **higher-order function** that abstracts common patterns.

-----

# ➡️ Higher-Order Functions: Passing Functions as Arguments

## 🎯 Learning Objectives

By the end of this lecture, you will be able to:

  * Understand the utility of **higher-order functions (HOFs)** for code abstraction and reuse.
  * Apply the pattern of creating a HOF to consolidate similar, repetitive code segments.
  * Implement a generalized function that takes an operation function as an argument.

-----

## 🏗️ Part I: Abstraction with Higher-Order Functions

### 1\. The Core Idea

A **higher-order function (HOF)** accepts one or more functions as arguments, making it highly **parameterizable** and reusable.

  * **Motivation:** If you have $N$ functions that share a similar structure (e.g., they all recurse $N$ times or traverse a list), you can abstract the common structure into a single HOF.
  * **Mechanism:** The HOF contains the common logic (e.g., the recursion, the pattern matching), while the passed-in function argument captures the **unique operation** for that specific task.

### 2\. The Repetitive Pattern

Consider three simple, repetitive functions:

1.  **`increment_n_times(n, x)`:** Returns $x + n$.
2.  **`double_n_times(n, x)`:** Returns $x \cdot 2^n$.
3.  **`nth_tail(n, xs)`:** Returns the $n^{th}$ tail of the list $xs$.

All functions share this recursive structure:

```sml
fun F(0, x) = x
|   F(n, x) = OPERATION( F(n-1, x) )
```

-----

## 🧩 Part II: Creating the Higher-Order Function (`n_times`)

The common structure is **"apply a function $n$ times."** This is abstracted into a single HOF: `n_times`.

### 1\. The HOF Definition

The function `n_times` takes the operation function (`f`) as an argument, along with the iteration count (`n`) and the initial value (`x`).

```sml
fun n_times (f, n, x) =
    if n = 0 then
        x
    else
        f (n_times (f, n-1, x)) 
```

  * **Analysis:** The recursive call is NOT a tail call because the function `f` must be applied *after* the recursive call returns.

### 2\. Type of `n_times`

The inferred type of `n_times` is generally **polymorphic** and potentially complex, reflecting its generality:

$$
('a \rightarrow 'a) \times int \times 'a \rightarrow 'a
$$  * **Interpretation:**
* The operation function `f` must take an `'a` and return an `'a`.
* The initial value `x` must be an `'a'`.
* The overall result is an `'a'`.

-----

## 🔄 Part III: Implementing Concrete Functions

Concrete, first-order functions are now easily implemented by simply calling the HOF `n_times` and passing the specific operation function.

### 1\. Helper Operations

These functions define the unique work to be done at each recursive step:

```sml
fun increment x = x + 1;
fun double x = 2 * x;
fun triple x = 3 * x;
fun get_tail xs = case xs of _::tl => tl | _ => []; (* Tail is ::, NOT the function name *)
```

### 2\. Concrete Function Definitions

The original repetitive functions are replaced with concise calls to `n_times`:

```sml
(* The caller doesn't need to know it uses a HOF *)
fun addition n x = n_times(increment, n, x); 
fun double_n_times n x = n_times(double, n, x);
fun nth_tail n xs = n_times(get_tail, n, xs); 
```

**Result:** This approach leads to significant **code reuse** and makes the code more **extensible**. If a new operation (like `triple_n_times`) is needed, only a tiny helper function and a new wrapper call are required.


This segment discusses the **polymorphism** often found in higher-order functions (HOFs) like `n_times`, clarifies how to interpret these complex types, and formally distinguishes polymorphism from HOFs using concrete examples.

---

# 🔎 Higher-Order Functions: Understanding Types and Polymorphism

## 🎯 Learning Objectives

By the end of this lecture, you will be able to:

- Interpret the **polymorphic type** of a HOF, like `n_times`.
    
- Explain the constraints imposed by type variables (`'a`, `'b'`) within a HOF's signature.
    
- Recognize that higher-order functions and polymorphism are **separate concepts**.
    

---

## 🏗️ Part I: The Type of `n_times`

The higher-order function `n_times` applies a given function $F$ to a value $X$, $N$ times. Its type is constrained by the way its arguments are used in the function body.

### 1. Inferred Polymorphic Type

The actual type inferred for `n_times` is:

$$('a \rightarrow 'a) \times int \times 'a \rightarrow 'a$$

### 2. Constraints and Interpretation

The use of the single type variable **`'a`** (alpha) imposes the following constraints, ensuring the function can be composed with itself $N$ times:

|**Component**|**Constraint**|**Reason from Code (f (n_times (f, n-1, x)))**|
|---|---|---|
|**Function `f`**|$\mathbf{'a \rightarrow 'a}$|The argument type must match the result type, because the result of `n_times` (an `'a'`) is passed back to `f` in the recursive step.|
|**Initial Value `x`**|$\mathbf{'a}$|Must match the argument type of `f` and the overall return type (since it's returned when $n=0$).|
|**Overall Result**|$\mathbf{'a}$|Must match the type of the initial value `x` and the result of `f`.|
|**Count `n`**|$\mathbf{int}$|Used in the base case check ($n=0$) and in the recursive step ($n-1$).|

### 3. Reusability through Polymorphism

The polymorphic nature allows `n_times` to be used for different types by **instantiating `'a'` consistently**:

- **`double_n_times`:** `'a'` is instantiated as **`int`**.
    
    - $int \rightarrow int \times int \times int \rightarrow int$
        
- **`nth_tail`:** `'a'` is instantiated as **`'b' list`** (e.g., `'int list'`).
    
    - $('b \text{ list} \rightarrow 'b \text{ list}) \times int \times 'b \text{ list} \rightarrow 'b \text{ list}$
        

> **Benefit:** Without polymorphism, you would need to write separate versions of `n_times` for integers, string lists, boolean lists, etc., undermining the goal of reusable HOFs.


## 🔗 Part II: HOFs and Polymorphism are Separate Concepts

It's important to recognize that while polymorphism is common in HOFs, the two concepts are not interdependent.

### 1. Higher-Order, Non-Polymorphic Functions

A function can take another function as an argument yet be limited to a specific type.

- **Example: `times_until_zero(f, x)`**
    
    - **Goal:** Count how many times $f$ must be applied to $x$ until the result is 0.
        
    - **Type:** $(int \rightarrow int) \times int \rightarrow int$
        
    - **Reason:** The use of the explicit comparison `x = 0` **constrains** $x$ (and therefore $f$'s domain/range) to be `int`. It is an HOF, but **not polymorphic**.
        

### 2. First-Order, Polymorphic Functions

A function can be highly polymorphic without taking any function arguments.

- **Example: `length(xs)`**
    
    - **Goal:** Compute the number of elements in a list.
        
    - **Type:** $\mathbf{'a \text{ list} \rightarrow int}$
        
    - **Reason:** The function never operates on or inspects the elements, only the structure of the list. It is **polymorphic**, but **not higher-order**. 




This segment introduces **anonymous functions** in SML, a language construct that allows functions to be defined as expressions without a name. This is particularly useful when passing functions to higher-order functions.

-----

# 📝 Anonymous Functions (`fn`)

## 🎯 Learning Objectives

By the end of this lecture, you will be able to:

  * Understand the motivation for using anonymous functions: better style and localized scope.
  * Define an anonymous function using the `fn` keyword.
  * Differentiate between `fun` bindings and `fn` expressions.
  * Understand the limitation of anonymous functions (they cannot be recursive).

-----

## 💡 Part I: Motivation and Progression

Anonymous functions provide the best way to define a function only where it is needed, usually as an argument to a higher-order function.

### The Problem: Over-scoping a Helper Function

Consider the function `triple_n_times(n, x)` which uses the HOF `n_times`:

1.  **Version 1 (Poor Style):** Defining the helper function `triple` at the top level, giving it unnecessarily wide scope.

    ```sml
    fun triple x = 3 * x; 
    fun triple_n_times n x = n_times(triple, n, x); 
    ```

2.  **Version 2 (Better Style):** Using a `let` binding to restrict the scope of `triple` to only `triple_n_times`.

    ```sml
    fun triple_n_times n x =
        let fun triple y = 3 * y
        in n_times(triple, n, x)
        end;
    ```

3.  **Version 3 (Better Scope):** Restricting the scope of `triple` to only the first argument position of `n_times`. (This is syntactically awkward but works because `let` is an expression.)

    ```sml
    fun triple_n_times n x =
        n_times( (let fun triple y = 3 * y in triple end) , n, x);
    ```

### The Solution: Anonymous Functions (`fn`)

The goal is to define the function without a name right where it's passed. Since the `fun` binding is a declaration, not an expression, we use the `fn` expression:

```sml
fun triple_n_times n x =
    n_times( fn y => 3 * y , n, x);
```

-----

## 🔑 Part II: Syntax and Properties

### 1\. Anonymous Function Syntax

The `fn` construct defines a function expression:

$$
\mathbf{fn} \ P \Rightarrow E
$$  * **`fn`:** The keyword (no 'u').
* **$P$ (Pattern):** The formal argument pattern (often just a single variable, e.g., `y`).
* **`=>` (Double Arrow):** Separates the argument from the body.
* **$E$ (Body):** The expression to be returned.

**Example:** `fn y => 3 * y` is a function that takes one argument `y` and returns `3 * y`.

### 2\. Primary Use Case

The most common use of anonymous functions is defining simple, single-use functions to be passed as arguments to a **higher-order function**. Since the HOF already gives the function a name (e.g., `f` in `n_times(f, ...)`), a local name is redundant.

### 3\. The Recursion Limitation 🚫

**Anonymous functions cannot be recursive.**

* **Reason:** Recursive functions call themselves by name. Since an anonymous function does not have a name, there is no way for the function body to refer to itself.
* **Rule:** If a function needs to be recursive, it **must** be defined using the `fun` binding (at top level or inside a `let`).

### 4\. `fun` vs. `val`/`fn`

For non-recursive functions, the `fun` binding is often considered **syntactic sugar** for an equivalent `val` binding using an anonymous function:

| `fun` Binding (Better Style) | Equivalent `val`/`fn` Binding |
| :--- | :--- |
| `fun triple x = 3 * x;` | `val triple = fn x => 3 * x;` |
| `fun f (n, x) = E;` | `val f = fn (n, x) => E;` |

While `val`/`fn` means the same thing, the `fun` binding is generally preferred for named functions because it is shorter and easier to read. The `fn` expression is reserved for short, unnamed functions used in place.




Now that you know how to use anonymous functions, here's a crucial point on style: avoiding the overuse of them.

-----

# 🛑 Avoiding Unnecessary Function Wrapping

## 🎯 Learning Objectives

  * Recognize and avoid the **unnecessary function wrapping** pattern.
  * Understand that passing a function directly by name is usually superior to wrapping it in an anonymous function.
  * Apply direct function binding (`val f = g`) to create aliases for existing functions.

-----

## 🌯 Part I: The Unnecessary Function Wrapping Pattern

This pattern occurs when an anonymous function is used to wrap a function that already exists and performs the exact desired operation.

### 1\. The Inferior Style

When using a Higher-Order Function (HOF) like `n_times`, it's tempting to use an anonymous function to pass a simple, existing operation:

```sml
fun nth_tail n xs =
    n_times( fn y => tail y , n, xs); (* Unnecessary Wrapping *)
```

  * **What the anonymous function does:** It takes an argument (`y`) and immediately passes it to the function `tail`, returning the result.
  * **Why it's inferior:**
      * **Redundancy:** It introduces an extra, unnecessary function definition.
      * **Performance:** It adds a slight overhead by requiring two function calls (the anonymous function calls `tail`) instead of one.
      * **Obscurity:** It hides the fact that the built-in `tail` function already does exactly what's needed.

### 2\. The Superior Style

The superior approach is to pass the existing function **directly by name** as the argument to the HOF.

```sml
fun nth_tail n xs =
    n_times(tail, n, xs); (* Direct Function Use *)
```

### General Analogy

The pattern is similar to writing redundant boolean logic:

| Redundant Expression | Simpler Equivalent | Concept |
| :--- | :--- | :--- |
| `if X then true else false` | `X` | Redundant conditional logic. |
| `fn y => F y` | `F` | Redundant function wrapper. |

-----

## 🤝 Part II: Creating Function Aliases

The same "unnecessary wrapping" pattern can occur when creating a new, simpler name (an **alias**) for an existing library function.

### 1\. The Wrapping Alias (Inferior)

Defining an alias by wrapping the original function in a new binding:

```sml
(* Suppose list.rev is in the standard library *)
fun rev xs = List.rev xs; (* Unnecessary Wrapping *)
```

This is equivalent to `val rev = fn xs => List.rev xs;`. This defines a *new* function that just calls `List.rev`.

### 2\. Direct Function Binding (Superior)

The simplest and most efficient way to create an alias is to use a **`val` binding** to bind the new name directly to the function value.

```sml
val rev = List.rev; (* Direct Function Alias *)
```

  * **What this does:** It evaluates the expression `List.rev` (which is a function value) and binds that same function value to the variable `rev`.
  * **Benefits:** It's **shorter**, **more direct**, and has **zero overhead** compared to the original function, as it does not involve an extra function call.

> **Key Takeaway:** When using a HOF or creating an alias, if your new or anonymous function's only job is to take its arguments and pass them straight to another function, **just pass the original function by name instead.**



This segment introduces **Map** and **Filter**, two of the most fundamental and famous **higher-order functions (HOFs)** used across functional programming languages. They provide standard, reusable patterns for processing collections like lists.

-----

# 🗺️ Higher-Order Functions: Map and Filter

## 🎯 Learning Objectives

  * Define the structure and purpose of the **`map`** HOF.
  * Define the structure and purpose of the **`filter`** HOF.
  * Understand the polymorphic types and constraints of both functions.
  * Use `map` and `filter` to write concise, abstract code.

-----

## 🚀 Part I: The `map` Function

**Purpose:** `map` applies a function to **every element** in a list and produces a new list of the **same size and shape**.

### 1\. Definition

Map takes two arguments: an operation function (`f`) and a list (`xs`).

```sml
fun map (f, xs) =
    case xs of
        [] => []
    |   x::xs' => (f x) :: (map (f, xs'))
```

  * **Logic:**
      * **Base Case:** Mapping over an empty list yields an empty list.
      * **Recursive Step:** The result is the new head (`f x`) cons'd onto the result of mapping the rest of the list (`xs'`).

### 2\. Polymorphic Type

The key feature of `map` is that it can change the type of the elements.

$$
('a \rightarrow 'b) \times 'a \text{ list} \rightarrow 'b \text{ list}
$$  * **Interpretation:**
* `f`: A function that takes an element of type **`'a`** and returns an element of type **`'b`**.
* `xs`: A list of **`'a`** elements.
* Result: A list of **`'b`** elements.

| Example | Function (`'a` to `'b'`) | Input List (`'a' list`) | Output List (`'b' list`) |
| :--- | :--- | :--- | :--- |
| **Increment** | `int -> int` (`'a'` = `'b'` = `int`) | `[4, 8, 12]` | `[5, 9, 13]` |
| **Head** | `int list -> int` (`'a'` = `int list`, `'b'` = `int`) | `[[1,2],[3,4]]` | `[1, 3]` |

> **Style Note:** Using `map` clearly communicates the intent to transform every element of a collection, which improves code readability.

-----

## 🧹 Part II: The `filter` Function

**Purpose:** `filter` uses a predicate function to select a **subset** of elements from a list, preserving their original order.

### 1\. Definition

Filter takes two arguments: a predicate function (`f`) and a list (`xs`).

```sml
fun filter (f, xs) =
case xs of
[] => []
|   x::xs' =>
if f x then
x :: (filter (f, xs')) (* Keep x *)
else
filter (f, xs')        (* Discard x *)
```

* **Logic:**
* **Base Case:** Filtering an empty list yields an empty list.
* **Recursive Step:** If the predicate `(f x)` is true, the head `x` is kept; otherwise, it's discarded. Recursion continues on the rest of the list (`xs'`).

### 2\. Polymorphic Type

Filter does *not* change the type of the elements.

$$('a \\rightarrow bool) \\times 'a \\text{ list} \\rightarrow 'a \\text{ list}
$$  \* **Interpretation:**
\* `f`: Must be a **predicate** function, taking an element of type **`'a`** and returning a **`bool`**.
\* `xs`: A list of **`'a`** elements.
\* Result: A list of **`'a`** elements (a subset of the input).

### 3\. Example Usage

To get only the even numbers from a list:

```sml
fun is_even x = (x mod 2) = 0;

fun all_even xs = filter (is_even, xs);

val result = all_even [3, 4, 6, 0, 13]; (* Result: [4, 6, 0] *)
```

> **Vocabulary:** Both `map` and `filter` are canonical examples of HOFs and are essential tools in the functional programming paradigm.



This segment emphasizes the **generality** of first-class functions beyond simple list processing (like `map` and `filter`). It demonstrates two advanced uses: functions returning other functions, and defining HOFs over custom, recursive data structures.

-----

# 🌐 Generalizing First-Class Functions

## 🎯 Learning Objectives

  * Recognize the full generality of first-class functions (as arguments, results, and data structure members).
  * Implement a function that **returns another function** as its result.
  * Understand the right-associativity of the function arrow (`->`) in SML types.
  * Define a **Higher-Order Function (HOF)** over a custom, recursive data type (e.g., an arithmetic expression tree).

-----

## ↩️ Part I: Functions Returning Functions

First-class functions can occupy the result position in a function type, allowing a function to select and return a specialized function.

### 1\. Example: `double_or_triple`

This function takes a predicate $F$ and returns either a "doubling" or a "tripling" function based on $F$'s result when applied to `7`.

```sml
fun double_or_triple f =
    if f 7 then
        fn x => 2 * x (* Returns the doubling function *)
    else
        fn x => 3 * x (* Returns the tripling function *)
```

### 2\. Type Inference and Associativity

The inferred type of `double_or_triple` is:

$$
(int \rightarrow bool) \rightarrow (int \rightarrow int)
$$The SML REPL often prints this without parentheses: `(int -> bool) -> int -> int`.

* **Function Arrow ($\rightarrow$) Associativity Rule:** The function arrow is **right-associative**.
* **Interpretation:** A type $T_1 \rightarrow T_2 \rightarrow T_3$ is implicitly grouped as $T_1 \rightarrow (T_2 \rightarrow T_3)$.
* The function takes $T_1$ and **returns a function** of type $T_2 \rightarrow T_3$.
* **In the Example:** `double_or_triple` takes an `(int -> bool)` and returns an `(int -> int)`.

-----

## 🌳 Part II: HOFs over Custom Data Types

HOFs are excellent for abstracting common recursive traversal logic over **any recursive data structure**, not just built-in lists.

### 1\. The Custom Data Type (Arithmetic Expressions)

Assume a data type for arithmetic expressions:

```sml
datatype exp = Const of int
| Plus of exp * exp
| Times of exp * exp;
```

### 2\. Example HOF: `true_of_all_constants` (Higher-Order Predicate)

This HOF performs a complete recursive traversal of the expression tree, applying a given predicate function $F$ only to the leaf **`Const`** nodes.

```sml
fun true_of_all_constants (f, e) =
case e of
Const i => f i (* Apply F only to constants *)
|   Plus (e1, e2) => 
true_of_all_constants(f, e1) andalso true_of_all_constants(f, e2)
|   Times (e1, e2) => 
true_of_all_constants(f, e1) andalso true_of_all_constants(f, e2)
```

* **Purpose:** It abstracts the complex recursive traversal (the "how to look for constants") so users only need to define the simple test (the "what to test").

### 3\. Usage (Concrete Predicate)

The HOF is then used to solve specific problems concisely:

```sml
(* Checks if all constants in the expression E are even *)
fun all_constants_even e = 
let fun is_even i = (i mod 2) = 0 
in true_of_all_constants(is_even, e)
end
```

### 4\. HOF Type and Terminology

The inferred type reflects the abstraction:

$$(int \\rightarrow bool) \\times exp \\rightarrow bool
$$  \* **Terminology:** A function that takes an expression and returns a boolean is called a **Predicate** (a nice mathematical term for a true/false condition). `true_of_all_constants` is a **Higher-Order Predicate** because it takes another predicate as an argument.



This segment introduces **Lexical Scope**, a fundamental concept for understanding the full power of first-class functions and how they interact with variables that are not passed as arguments.

---

# 📚 Lexical Scope and Function Closures

## 🎯 Learning Objectives

* Define **Lexical Scope** and distinguish it from other possible scoping rules.
* Understand the role of the **function closure** in implementing Lexical Scope.
* Predict the value of a variable based on the environment where the function was *defined*, not where it was *called*.

---

## 🧐 Part I: Lexical Scope

**Lexical Scope** is the rule used by ML (and most modern programming languages) to determine which environment a function uses to look up variables that are **not** its arguments and **not** locally defined.

* **The Rule:** A function uses the environment (scope) that was active **when the function was defined (created)**, not the environment that is active when the function is called.
* **Intuition:** The function "remembers" the surrounding context where it was written, regardless of where it travels in the program.

### Example Walkthrough (Non-HOF)

Consider the following sequence:

1.  `val x = 1;` $\implies$ Environment $E_1$: $x \mapsto 1$.
2.  `fun f y = x + y;` $\implies$ Function $f$ is defined in $E_1$. $f$'s code will *always* look up $x$ in $E_1$.
3.  `val x = 2;` $\implies$ Environment $E_2$: $x \mapsto 2$. **(This shadows the previous $x$)**
4.  `val y = 3;` $\implies$ Environment $E_3$: $x \mapsto 2, y \mapsto 3$.
5.  `val z = f(x + y);` $\implies$ **Call Site:** $x+y$ evaluates to $2+3=5$. $f$ is called with argument $5$.
    * **Inside $f$'s body (`x + y`):**
        * The argument $y$ is $5$.
        * The free variable $x$ is looked up in $f$'s **definition environment** ($E_1$), where $x \mapsto 1$.
    * Result: $1 + 5 = 6$.
6.  `val z = 6;`

> If the result were 7 (from $2 + 5$), the language would be using **Dynamic Scope**, which is non-standard and rarely used today.

---

## 🔒 Part II: Function Closures

To implement Lexical Scope, the function value itself must carry the definition environment along with the code. This pair is called a **closure**.

### The Structure of a Function Value

A function value in ML is actually a **Closure**, which is a pair of two components:

1.  **Code:** The actual function body/instructions (e.g., `fn y => x + y`).
2.  **Environment (Env):** A pointer to the complete environment that was active when the function was created.

$$\text{Function Value} = \text{Closure} = (\text{Code}, \text{Environment})$$

### Semantics of a Function Call

When a function is called, the steps are:

1.  A new, temporary environment is created, extending the **Closure's Environment** with the argument bindings (e.g., $y \mapsto 5$).
2.  The **Closure's Code** is executed within this newly extended environment.

The closure ensures that variables defined outside the function (like $x$ in the example) are resolved using the correct (defining) scope, even if that scope has been modified or exited in the interim.

---

I can now show you how this powerful feature enables advanced programming techniques, such as creating functions on the fly that remember specific data. Would you like to see examples of how **Lexical Scope** is used in conjunction with **first-class functions** to create powerful programming idioms?



This segment reinforces the concept of **Lexical Scope** in the context of **higher-order functions (HOFs)**, where functions are passed as arguments or returned as results. The core rule remains unchanged, but the interaction with nested scopes becomes more complex and interesting.

-----

# 🔎 Lexical Scope in Higher-Order Functions

## 🎯 Learning Objectives

  * Apply the **Lexical Scope rule** correctly when dealing with nested functions and HOFs.
  * Understand that the environment associated with a function's closure is determined **at the point of definition**, regardless of subsequent variable shadowing or function calls.

-----

## 🔑 Part I: The Unchanging Rule of Lexical Scope

The fundamental rule of **Lexical Scope** does not change when introducing HOFs:

> **The Rule:** A function's body is **always** evaluated using the **environment (closure)** that was active when the function was defined.

The complexity now arises because the definition environment might contain bindings for variables that are later passed into, or returned from, other functions.

-----

## 🔄 Part II: Example 1: Function Returning a Function

This example shows a function (`f`) that creates and returns a specialized **closure** (`fn z => ...`), which captures variables from `f`'s local scope.

```sml
val x = 1;

fun f y = (* F is defined with outer x=1 in scope *)
    let val x = y + 1 (* Local x shadows outer x; x = 5 when y=4 *)
    in fn z => x + y + z (* This closure captures: x (local) and y (argument) *)
    end

(* Call and Usage *)
val x = 3; (* Irrelevant shadow *)
val g = f 4; (* Call f with y=4. Returns a closure g. *)
val z = g 6; (* Call g with z=6. *)
```

### **Evaluation Step-by-Step:**

1.  **Define `f`:** $f$ is a function defined at top level.
2.  **Call `f 4`:** The argument is $y=4$.
      * Inside $f$, the local environment is extended: $y \mapsto 4$.
      * The inner binding is calculated: $x = y + 1 \implies x \mapsto 5$.
      * **Closure Creation:** The expression `fn z => x + y + z` is evaluated. It creates a **closure** ($g$) that captures the environment where $x \mapsto 5$ and $y \mapsto 4$.
3.  **Call `g 6`:** The closure $g$ is called with argument $z=6$.
      * **Evaluation:** $g$ uses its captured environment ($x \mapsto 5, y \mapsto 4$) and the argument ($z \mapsto 6$).
      * **Result:** $x + y + z = 5 + 4 + 6 = 15$.

-----

## ➡️ Part III: Example 2: Function Taking a Function (As Argument)

This example shows a function (`f`) that calls an *already-defined* function (`h`), demonstrating that the called function retains its original defining environment.

```sml
fun f g = g 2; (* F calls its argument G with the constant 2 *)

val x = 4;

val h = fn y => x + y; (* H is defined at top level, capturing x=4 *)

(* Call *)
val z = f h;
```

### **Evaluation Step-by-Step:**

1.  **Define `h`:** The anonymous function `h` is defined at the top level. It creates a **closure** that captures the environment where $x \mapsto 4$.
      * $h$'s intent: "Take an argument $y$ and return $4 + y$."
2.  **Call `f h`:** $f$ receives the closure $h$ as its argument $g$.
3.  **Inside `f`:** $f$ executes `g 2`. This calls the closure $h$ with argument $y=2$.
4.  **Inside `h`'s body:** $h$ executes `x + y` using its captured environment ($x \mapsto 4$) and its argument ($y \mapsto 2$).
5.  **Result:** $x + y = 4 + 2 = 6$.

> **Crucial Insight:** The code within $f$ (which runs where $x$ might be undefined or shadowed) *cannot* influence the variable $x$ used by $h$. The variable $x$ is fixed by $h$'s **definition site**.



This segment provides the crucial **motivation** for using **Lexical Scope** over **Dynamic Scope**, demonstrating how Lexical Scope enables essential software engineering principles like modularity, type safety, and powerful programming idioms using closures.

-----

# 💡 Why Lexical Scope? The Motivation

## 🎯 Learning Objectives

  * Understand why **Lexical Scope** is the preferred and standard scoping rule in modern languages (like SML).
  * Contrast Lexical Scope with **Dynamic Scope** (using the call site environment).
  * Recognize the three major technical advantages of Lexical Scope: modularity, type checking, and powerful data encapsulation using closures.

-----

## ⚖️ Part I: Lexical vs. Dynamic Scope

| Feature | Lexical Scope (SML Default) | Dynamic Scope (Non-Standard) |
| :--- | :--- | :--- |
| **Scope Rule** | Look up variables in the environment where the function was **DEFINED** (the Closure's environment). | Look up variables in the environment where the function was **CALLED** (the current run-time environment). |
| **Status** | Standard, crucial for functional programming. | Obsolete for general variable lookup; only used for specialized features (e.g., exceptions). |

-----

## 🛡️ Part II: Technical Advantages of Lexical Scope

Lexical Scope enables three key principles of robust software design:

### 1\. Modularity and Abstraction

The meaning of a function is self-contained and isolated.

  * **Variable Renaming:** You can change the names of local variables (or free variables captured by the closure) without affecting any caller. If you change a variable `x` to `pizza` inside function $F$, $F$ still works correctly because it only uses the environment where it was defined.
      * *Dynamic Scope Failure:* If the caller had its own variable `x`, the function might suddenly use the caller's `x`, leading to unpredictable behavior and breaking modularity.
  * **Removing Unused Variables:** You can confidently remove unused local bindings (e.g., `let val x = 3 in ... end`) without worrying that some external, dynamically-scoped function might have relied on that variable being present at the call site.

### 2\. Static Type Checking and Reasoning

Lexical Scope allows a function to be fully type-checked **at its definition site**.

  * **Predictable Types:** When a function is defined, the types of all its free variables are fixed by the environment captured in the closure. This guarantees that the function will always execute with the expected types.
  * *Dynamic Scope Failure:* A function could be defined to use an integer variable $x$, but if called from a scope where $x$ is bound to a string, the program would fail at runtime, negating the benefits of static type checking.

### 3\. Data Encapsulation and Powerful Closures

Lexical Scope allows a closure to **store (encapsulate) necessary data** that is independent of the call site. This is the most powerful feature.

  * **Example: Parameterized Filtering:**
    We want a function that filters a list to include only numbers greater than a specific value $N$.

    ```sml
    (* HOF that returns a closure that captures N *)
    fun greater_than n = fn y => y > n; 

    (* Use: create a specialized filter function *)
    val non_negative_filter = greater_than (~1); 

    (* Pass the specialized closure to the HOF filter *)
    val result = filter(non_negative_filter, [5, ~2, 0, 10]); 
    ```

      * **Mechanism:** `non_negative_filter` is a closure that **permanently remembers** $n = -1$. When passed to `filter`, it correctly tests `y > -1` for every element.
      * *Dynamic Scope Failure:* Under Dynamic Scope, the inner function would try to look up $n$ at the call site (inside `filter`), where $n$ is probably undefined or bound to an irrelevant variable, making this powerful idiom impossible.

-----

## 📜 Part III: The Exception to the Rule

While Lexical Scope is the default for variables, some language features naturally follow a dynamic approach:

  * **Exceptions (e.g., SML `raise`/`handle`):** The search for an appropriate exception handler is **dynamic**. It follows the current **call stack** (the chain of active function calls) backward, not the lexical structure of the code where the exception was defined. This is generally accepted as the most convenient model for exception handling.