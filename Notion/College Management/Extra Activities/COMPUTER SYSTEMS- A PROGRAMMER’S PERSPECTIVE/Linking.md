Linking is the process of collecting and combining various pieces of code and data into a single file that can be loaded (copied) into memory and executed.

  

Linking can be performed at compile time, when the source code is translated into machine code; at load time, when the program is loaded into memory and executed by the loader; and even at run time, by application programs.

  

Linking is performed automatically by programs called linkers. Linkers enable separate compilation. Instead of organizing a large application as one monolithic source file, it can be decomposed into smaller, manageable modules. If changes happen to modules, we just recompile and relink without having to recompile other files.

  

# Compiler Drivers

### **What is a Compiler Driver?**

A **compiler driver** is a program that orchestrates the different phases of the compilation process in a compilation system. It acts as a **coordinator** that invokes various tools such as the **preprocessor, compiler, assembler, and linker** to transform source code into an executable.

For example, in GCC (`gcc`), the compiler driver determines what steps are needed based on the input file and user-specified flags. It calls:

- The **preprocessor** (e.g., `cpp`)
- The **compiler** (e.g., `cc1`)
- The **assembler** (e.g., `as`)
- The **linker** (e.g., `ld`)

### **Compilation Process in Compilation Systems**

The compilation process typically consists of the following phases:

1. **Preprocessing**
    - Handles preprocessor directives (`#include`, `#define`, etc.).
    - Expands macros and includes files.
    - Example: `gcc -E program.c -o program.i` (generates preprocessed output).
2. **Compilation (Translation to Assembly)**
    - Converts preprocessed C code into assembly language.
    - Example: `gcc -S program.i -o program.s` (generates assembly code).
3. **Assembly**
    - Translates assembly code to machine code (object file `.o`).
    - Example: `gcc -c program.s -o program.o`.
4. **Linking**
    - Combines object files with libraries to create an executable.
    - Example: `gcc program.o -o program` (creates `program` executable).

  

# Static Linking

- static linkers such as the Linux ld program take as input a collection of relocatable object files and command-line arguments and generate as output a fully linked executable object file that can be loaded and run.
- the input relocatable object files consist of various code and data sections, where each section is a contiguous sequence of bytes. instructions are in one section, initialized global variables are in another section, and uninitialized variables are in yet another section.

---

# **How the Linker Works: Symbol Resolution & Relocation**

Once the compiler and assembler have done their jobs and produced object files (`.o`), the **linker (**`**ld**`**)** steps in to combine these into a final executable. This process has two major tasks:

## **Step 1: Symbol Resolution**

### **What is a Symbol?**

A **symbol** is any **named entity** in a program, such as:

- **Functions** (e.g., `main`, `printf`)
- **Global variables** (e.g., `int counter;`)
- **Static variables** (e.g., `static int count = 0;`)

When compiling, each object file (`.o`) **defines** and **references** symbols.

### **Goal of Symbol Resolution**

The linker’s job is to **match each symbol reference with exactly one symbol definition**.

### **Example: Symbol Resolution**

Consider two source files:

### `**main.c**`

```C
\#include <stdio.h>
extern int count;  // Reference to a symbol (defined elsewhere)

int main() {
    printf("Count = %d\n", count);
    return 0;
}
```

### `**counter.c**`

```C
int count = 42;  // Definition of the symbol
```

### **Compilation Process**

```Plain
gcc -c main.c -o main.o      # main.o has an unresolved reference to `count`
gcc -c counter.c -o counter.o # counter.o defines `count`
gcc main.o counter.o -o program  # Linker resolves `count`
```

During **symbol resolution**, the linker:

1. Sees `main.o` **references** `count` (but doesn't define it).
2. Finds `counter.o` **defines** `count`.
3. Resolves `count` by linking `main.o` to `counter.o`.

If `count` were missing (not defined in any `.o` file), the linker would throw an **"undefined reference"** error.

---

## **Step 2: Relocation**

### **Why is Relocation Needed?**

- When compiling, each object file assumes it **starts at address** `**0**`.
- The linker must **adjust memory addresses** so that:
    - Functions and variables are placed at their **actual** locations in memory.
    - Instructions referring to these symbols are updated accordingly.

### **Two Types of Relocation**

### **1️⃣ Relocating Code Sections (**`**.text**`**)**

- Function calls use **relative addresses**.
- The linker adjusts the **offsets** to ensure calls jump to the correct addresses.

### **2️⃣ Relocating Data Sections (**`**.data**`**,** `**.bss**`**)**

- Global/static variables need actual memory addresses.
- The linker replaces placeholder addresses with real locations.

### **Example: Relocation**

Consider this simple function:

```C
void hello() {
    printf("Hello\n");
}
int main() {
    hello();
    return 0;
}
```

### **Assembly Before Relocation (**`**main.s**`**):**

```Plain
call 0x00000000  # Placeholder address for `hello`
```

### **After Relocation (**`**main.o**` **→** `**program**`**):**

```Plain
call 0x08048500  # Real address assigned by linker
```

The linker replaces `0x00000000` with the **actual memory address** where `hello()` is stored.

---

## **Relocation Entries: How the Linker Knows What to Fix**

When the assembler generates `.o` files, it includes **relocation entries**—instructions for the linker on **what to patch**.

You can view relocation entries with:

```Plain
readelf --relocs main.o
```

Example output:

```Plain
Relocation section '.rel.text':
  Offset     Info    Type            Symbol
  00000004   000004  R_X86_64_PC32   hello
```

This tells the linker:

1. At offset `0x00000004`, there's a reference to `hello`.
2. It must replace this with the **correct** address.

The linker processes these relocation entries and patches the final executable.

---

## **Static vs. Dynamic Linking and Their Impact on These Steps**

- **Static linking**: The linker performs **all** symbol resolution and relocation at build time, producing a **self-contained executable**.
- **Dynamic linking**: The linker **defers** some work (e.g., resolving `printf`) to runtime, requiring the **dynamic linker (**`**ld.so**`**)** to handle it when the program runs.

---

## **Final Thoughts**

- **Symbol Resolution** ensures every function/variable reference has a **unique** definition.
- **Relocation** adjusts memory addresses so everything **points to the right place**.
- The linker relies on **relocation entries** to modify addresses correctly.

Would you like a **deep dive into dynamic linking** and how the **runtime linker (**`**ld.so**`**) works**? 🚀

  

  

Object files are merely collections of blocks of bytes. Some of these blocks contain program code, others contain program data, and others contain data structures that guide the linker and loader. A linker concatenates blocks together, decides on run-time locations for the concatenated blocks, and modifies various locations within the code and data blocks. Linkers have minimal understanding of the target machine. The compilers and assemblers that generate the object files have already done most of the work.

  

  

# **Symbols and Symbol Tables in Object Files**

In **compilation and linking**, a **symbol** is a **name** (identifier) for a function, variable, or any other entity in a program. **Symbol tables** store mappings between these names and their **corresponding memory locations** (or placeholders before linking).

When a program is compiled, the **compiler** and **linker** use symbol tables to:

✅ Resolve **variable and function names**

✅ Manage **scope and linkage**

✅ Assist in **relocation** (adjusting memory addresses during linking)

---

## **🔹 Types of Symbols in Object Files**

Every function and variable declared in a program **corresponds to a symbol**. Symbols can be:

|   |   |   |
|---|---|---|
|**Category**|**Example**|**Where It’s Found**|
|**Functions**|`main()`, `printf()`|**Defined in .text**|
|**Global Variables**|`int x = 10;`|**.data or .bss**|
|**Static Variables**|`static int count;`|**Limited to the file**|
|**External Symbols**|`printf()` from `<stdio.h>`|**Defined elsewhere**|
|**Undefined Symbols**|Calls to `printf()` in `main.o`|**Need linking**|

---

## **🔍 Inspecting the Symbol Table**

We can view the **symbol table** in an object file using `readelf` or `nm`.

### **1️⃣ Generating an Object File**

```C
// file: main.c
\#include <stdio.h>

int global_var = 42;  // Global variable (stored in .data)

static int static_var = 10; // Static variable (not exposed outside this file)

void func() {
    printf("Hello\n");
}

int main() {
    func();
    return 0;
}
```

Compile without linking:

```Plain
gcc -c main.c -o main.o
```

### **2️⃣ Viewing Symbols (**`**readelf -s**`**)**

```Plain
readelf -s main.o
```

Example output:

```Plain
Symbol table '.symtab' contains 7 entries:
  Num:  Value   Size Type    Bind   Vis  Name
    1: 00000000    0 FILE    LOCAL  DEFAULT  main.c
    2: 00000000    4 OBJECT  GLOBAL DEFAULT  global_var
    3: 00000000    4 OBJECT  LOCAL  DEFAULT  static_var
    4: 00000010    0 FUNC    GLOBAL DEFAULT  func
    5: 00000020    0 FUNC    GLOBAL DEFAULT  main
    6: 00000000    0 NOTYPE  GLOBAL DEFAULT  printf
```

### **Understanding the Fields**

|   |   |
|---|---|
|**Field**|**Meaning**|
|**Num**|Symbol index in the table.|
|**Value**|Address (or placeholder if not yet linked).|
|**Size**|Memory size of the symbol.|
|**Type**|`FUNC` (function), `OBJECT` (variable), `NOTYPE` (external).|
|**Bind**|`GLOBAL` (accessible across files), `LOCAL` (file scope).|
|**Vis**|Visibility (usually `DEFAULT`).|
|**Name**|Symbol’s identifier.|

---

## **🔹 Symbol Types in Detail**

### **1️⃣ Local Symbols (**`**LOCAL**`**)**

- **Not visible to the linker outside the file.**
- Includes:
    - `static` functions/variables.
    - **Section labels** (e.g., `.text`, `.data`).

✅ **Example: Static Variables**

```C
static int x = 10; // Only visible in this file
```

This **does not appear in the final executable** because it’s not **exported**.

---

### **2️⃣ Global Symbols (**`**GLOBAL**`**)**

- **Visible across multiple object files**.
- Includes:
    - Functions like `main()`, `func()`.
    - Global variables (unless `static`).

✅ **Example: Global Variables**

```C
int y = 100; // Can be used in other files
```

This **appears in the final executable** and can be accessed from other `.o` files.

---

### **3️⃣ Undefined Symbols (**`**NOTYPE**`**)**

- Symbols **not yet defined** in the current object file.
- The linker must **resolve these** by finding the correct definition.

✅ **Example: External Function Calls**

```C
void func() {
    printf("Hello\n"); // printf is undefined in main.o
}
```

Output in `readelf -s main.o`:

```Plain
6: 00000000    0 NOTYPE  GLOBAL DEFAULT  printf
```

🔸 `printf` is **undefined** because it's part of `libc` (linked later).

---

## **🔹 Symbol Resolution & Linking**

During **linking**, the **linker** resolves symbols:  
1️⃣ **Merges symbol tables from multiple** `**.o**` **files**.

2️⃣ **Resolves undefined symbols (e.g., printf)**.

3️⃣ **Replaces placeholder addresses with real ones**.

### **Example: Linking Two Object Files**

### **File 1: main.c**

```C
extern void helper();
int main() {
    helper();
    return 0;
}
```

Compile:

```Plain
gcc -c main.c -o main.o
```

### **File 2: helper.c**

```C
\#include <stdio.h>
void helper() {
    printf("Helper function\n");
}
```

Compile:

```Plain
gcc -c helper.c -o helper.o
```

Now, linking:

```Plain
gcc main.o helper.o -o program
```

The linker **resolves** `**helper()**` by matching the symbol in `helper.o`.

---

## **🔹 Common Symbol Errors**

### **1️⃣ Undefined Reference (**`**ld: undefined reference**`**)**

```Plain
gcc main.o -o program
```

⛔ **Error**:

```Plain
ld: main.o: undefined reference to `helper'
```

✅ Fix: Link `helper.o`:

```Plain
gcc main.o helper.o -o program
```

---

### **2️⃣ Multiple Definition (**`**ld: multiple definition**`**)**

### **File 1: a.c**

```C
int x = 10; // Global variable
```

### **File 2: b.c**

```C
int x = 20; // Same variable name
```

Compile and link:

```Plain
gcc a.o b.o -o program
```

⛔ **Error**:

```Plain
ld: multiple definition of `x'
```

✅ Fix:

- Use `static int x = 10;` to keep `x` local.
- Use `extern int x;` to declare but not redefine.

---

## **🔹 Dynamic Symbols (**`**.dynsym**` **vs** `**.symtab**`**)**

In **shared libraries (**`**.so**`**)**, symbols are stored in `.dynsym`, a stripped-down symbol table.

```Plain
readelf --dyn-syms /lib/x86_64-linux-gnu/libc.so.6
```

Shows only **exported symbols** (used for dynamic linking).

---

## **🚀 Key Takeaways**

✔ **Symbols represent variables, functions, and external references**.

✔ **Symbol tables help with linking, relocation, and debugging**.

✔ **Undefined symbols** are resolved at **link time**.

✔ `**LOCAL**` **vs** `**GLOBAL**` determines visibility across files.

✔ **Use** `**readelf -s**` **and** `**nm**` **to inspect symbols in object files**.

Would you like a **deep dive into dynamic linking and symbol resolution at runtime**? 🚀

  

  

# **🔍 Symbol Resolution: How the Linker Matches Symbols**

## **🔹 What is Symbol Resolution?**

**Symbol resolution** is the process where the **linker** assigns **each symbol reference to a unique symbol definition** during **linking**. This ensures that **variables, functions, and other identifiers** in an object file are correctly linked to their actual memory addresses.

For example, when a function like `printf()` is used in your program, the linker must find its **real definition** in the C standard library (`libc.so` or `libc.a`).

---

## **🔹 When Does Symbol Resolution Happen?**

Symbol resolution occurs in **two main phases** of the **linking process**:

1️⃣ **Static Linking (Compile-Time Resolution)**

- The linker **resolves all symbols** at link-time by finding them in object files and static libraries.
- Produces a **fully self-contained** executable (no external dependencies).

2️⃣ **Dynamic Linking (Run-Time Resolution)**

- The linker **leaves some symbols unresolved** and defers resolution until the program is loaded into memory.
- These symbols are resolved by the **dynamic linker (**`**ld.so**`**)** at runtime.

---

## **🔹 Symbol Resolution in Static Linking**

### **1️⃣ Example: Two Object Files**

We define a function in `helper.c` and use it in `main.c`.

### **File 1: main.c**

```C
\#include <stdio.h>
extern void helper();  // Function declaration

int main() {
    helper();  // Reference to helper()
    return 0;
}
```

Compile without linking:

```Plain
gcc -c main.c -o main.o
```

### **File 2: helper.c**

```C
\#include <stdio.h>
void helper() {  // Function definition
    printf("Helper function\n");
}
```

Compile:

```Plain
gcc -c helper.c -o helper.o
```

### **2️⃣ Linking: Resolving** `**helper()**`

Now, we link both `.o` files:

```Plain
gcc main.o helper.o -o program
```

**What happens here?**

- The linker looks at `main.o` and sees an **undefined reference to** `**helper()**`.
- It then **searches** `helper.o` for `helper()` and **matches the symbol**.
- The linker **binds** `**helper()**` to its definition and replaces the placeholder in `main.o` with the correct address.

If we forget to link `helper.o`, we get an error:

```Plain
ld: main.o: undefined reference to `helper'
```

✅ **Fix:** Include `helper.o` during linking.

---

## **🔹 Symbol Resolution in Dynamic Linking**

Unlike **static linking**, where symbols are fully resolved at compile time, **dynamic linking** defers some resolution until **runtime**.

### **1️⃣ Example: Calling** `**printf()**`

### **File: main.c**

```C
\#include <stdio.h>

int main() {
    printf("Hello, world!\n");  // printf is an external symbol
    return 0;
}
```

Compile and link dynamically:

```Plain
gcc main.c -o program
```

What happens?

- The compiler **sees** `**printf()**` and marks it as an **undefined symbol**.
- The linker **does not resolve** `**printf()**` at link time. Instead, it **records a dynamic symbol reference** to `libc.so`.
- At runtime, the **dynamic linker (**`**ld.so**`**)** loads `libc.so` and resolves `printf()`.

We can check which shared libraries are used:

```Plain
ldd program
```

Example output:

```Plain
linux-vdso.so.1 (0x00007fff5f7fd000)
libc.so.6 => /lib/x86_64-linux-gnu/libc.so.6 (0x00007f8c56c00000)
```

Here, `printf()` is found in `libc.so.6`, and its address is **resolved at runtime**.

---

## **🔹 How the Linker Resolves Symbols**

The linker uses **two main data structures** for symbol resolution:

1️⃣ **Symbol Table (**`**.symtab**`**)**

- Stores **function names, variables, and addresses**.
- Used by **both the compiler and linker**.
- Found in **object files, executables, and shared libraries**.

2️⃣ **Relocation Table (**`**.rela.text**`**)**

- Contains **entries for symbols that need address adjustments**.
- Used when merging object files or linking against shared libraries.

Example:

```Plain
readelf -s main.o
```

```Plain
Symbol table '.symtab' contains:
  Num: Value  Size   Type    Bind   Name
    1: 000000  0000  NOTYPE  GLOBAL printf
```

Since `printf` is `NOTYPE` (undefined), it **must be resolved by the linker**.

---

## **🔹 Handling Symbol Conflicts**

### **1️⃣ Multiple Definitions (Redefinition Errors)**

If a symbol is defined in **multiple files**, the linker **doesn't know which one to use**.

### **File 1: a.c**

```C
int x = 10;  // Global variable
```

### **File 2: b.c**

```C
int x = 20;  // Another global variable with the same name
```

```Plain
gcc -c a.c -o a.o
gcc -c b.c -o b.o
gcc a.o b.o -o program
```

⛔ **Error:**

```Plain
ld: multiple definition of `x'
```

✅ **Fix:**

- Use `static` to **restrict scope** (`static int x = 10;`).
- Use `extern` to **declare but not redefine** (`extern int x;`).

---

### **2️⃣ Undefined References**

If the linker **cannot find a symbol definition**, we get:

```Plain
ld: main.o: undefined reference to `some_function'
```

✅ **Fix:** Ensure that:

- The correct `.o` or `.so` file is linked.
- The function is **declared with** `**extern**` but **defined somewhere**.

---

## **🔹 Symbol Resolution in Shared Libraries**

If we compile a program dynamically, some symbols remain unresolved until **runtime**.

### **1️⃣ Example: Creating a Shared Library**

### **File: libmath.c**

```C
\#include <stdio.h>
void square(int x) {
    printf("%d squared is %d\n", x, x * x);
}
```

Compile into a shared library:

```Plain
gcc -shared -fPIC -o libmath.so libmath.c
```

### **File: main.c**

```C
extern void square(int);

int main() {
    square(5);
    return 0;
}
```

Compile dynamically:

```Plain
gcc main.c -o program -L. -lmath
```

Check dependencies:

```Plain
ldd program
```

```Plain
libmath.so => not found
```

⛔ **Fix:** Set `LD_LIBRARY_PATH`:

```Plain
export LD_LIBRARY_PATH=.
./program
```

---

## **🚀 Key Takeaways**

✔ **Symbol resolution matches symbol references with definitions**.

✔ **Static linking resolves all symbols at compile-time**.

✔ **Dynamic linking defers some resolution to runtime (**`**ld.so**`**)**.

✔ **The linker uses** `**.symtab**` **(symbol table) and** `**.rela.text**` **(relocation table)**.

✔ **Multiple definitions cause errors; undefined references must be resolved at link time**.

Would you like a deep dive into **symbol interposition and runtime symbol binding**? 🚀

  

  

# **🔗 Linking with Static Libraries (**`**.a**` **Files)**

## **🔹 What is a Static Library?**

A **static library** is a collection of precompiled object files (`.o` files) **bundled together** into a single archive (`.a` file). It allows code reuse without requiring recompilation of the original source code every time it's used.

📌 **Key Features of Static Libraries:**

✅ **Code is copied into the final executable** at link time.

✅ **No runtime dependencies** on the library file.

✅ Faster execution (no dynamic lookups).

✅ **Larger executable size** compared to dynamic libraries (`.so`).

---

## **🔹 Creating and Using a Static Library (**`**.a**` **file)**

### **1️⃣ Step 1: Write the Library Code**

Let’s create a **math library** with functions for square and cube.

### **File:** `**mathlib.c**`

```C
\#include <stdio.h>

void square(int x) {
    printf("%d squared is %d\n", x, x * x);
}

void cube(int x) {
    printf("%d cubed is %d\n", x, x * x * x);
}
```

### **File:** `**mathlib.h**` (Header File)

```C
\#ifndef MATHLIB_H
\#define MATHLIB_H

void square(int x);
void cube(int x);

\#endif
```

---

### **2️⃣ Step 2: Compile the Object File**

First, we compile `mathlib.c` into an object file (`.o` file):

```Plain
gcc -c mathlib.c -o mathlib.o
```

---

### **3️⃣ Step 3: Create a Static Library (**`**.a**` **file)**

Now, we use the `**ar**` **(archiver) command** to bundle `mathlib.o` into a static library:

```Plain
ar rcs libmath.a mathlib.o
```

📌 **Explanation of** `**ar rcs libmath.a mathlib.o**`**:**

- `**r**` – Replaces existing object files in the archive.
- `**c**` – Creates the archive if it doesn’t exist.
- `**s**` – Adds an index to make symbol lookup faster.
- `**libmath.a**` – The output archive (library).
- `**mathlib.o**` – The object file to include.

Verify the contents:

```Plain
ar -t libmath.a
```

```Plain
mathlib.o
```

---

### **4️⃣ Step 4: Use the Static Library in a Program**

Now, let’s write a program that **links against our static library**.

### **File:** `**main.c**`

```C
\#include <stdio.h>
\#include "mathlib.h"

int main() {
    square(5);
    cube(3);
    return 0;
}
```

---

### **5️⃣ Step 5: Compile and Link the Program**

To compile `main.c` and **link against** `**libmath.a**`, run:

```Plain
gcc main.c -L. -lmath -o program
```

📌 **Explanation:**

- `**L.**` → Look for libraries in the current directory (`.`).
- `**lmath**` → Link with `libmath.a` (the `lib` prefix is omitted).
- `**o program**` → Output executable is `program`.

Run the program:

```Plain
./program
```

Output:

```Plain
5 squared is 25
3 cubed is 27
```

---

## **🔹 Understanding How Static Linking Works**

### **What Happens at Link Time?**

When we link with `libmath.a`, the linker:  
1️⃣ **Finds unresolved symbols** in `main.o` (e.g., `square()`, `cube()`).

2️⃣ **Searches** `**libmath.a**` for those functions.

3️⃣ **Copies the required object files (**`**mathlib.o**`**)** into the final executable.

4️⃣ Produces a **self-contained binary** (no dependency on `libmath.a` after linking).

We can inspect the executable using:

```Plain
nm program | grep square
```

```Plain
0000000000001139 T square
```

This confirms `square()` is **statically linked** into `program`.

---

## **🔹 Static Libraries vs. Dynamic Libraries**

|   |   |   |
|---|---|---|
|**Feature**|**Static Library (**`**.a**`**)**|**Dynamic Library (**`**.so**`**)**|
|**Code Inclusion**|Copied into the executable|Loaded at runtime|
|**Executable Size**|Larger|Smaller|
|**Speed**|Faster (no runtime lookup)|Slower (needs runtime symbol resolution)|
|**Flexibility**|Less flexible (code is fixed at compile-time)|More flexible (can be updated separately)|
|**Runtime Dependency**|None (self-contained)|Yes (must have `.so` file)|

---

## **🔹 Manually Extracting** `**.o**` **Files from a Static Library**

We can extract object files from a static library using:

```Plain
ar x libmath.a
ls
```

Output:

```Plain
mathlib.o  libmath.a  main.c  mathlib.h
```

This allows us to modify object files before re-archiving.

---

## **🔹 Common Issues & Fixes**

### **1️⃣ Error:** `**undefined reference to 'square'**`

If we forget to link the library:

```Plain
gcc main.c -o program
```

⛔ **Error:**

```Plain
/usr/bin/ld: main.o: in function `main': undefined reference to `square'
```

✅ **Fix:** Link the library correctly:

```Plain
gcc main.c -L. -lmath -o program
```

---

### **2️⃣ Error: Wrong Library Order**

If we put `-lmath` **before** `main.c`:

```Plain
gcc -L. -lmath main.c -o program
```

⛔ **Error:** Symbols remain unresolved.

✅ **Fix:** Libraries should be **linked after object files**:

```Plain
gcc main.c -L. -lmath -o program
```

---

## **🚀 Key Takeaways**

✔ **Static libraries (**`**.a**` **files) bundle** `**.o**` **files for reuse.**

✔ **They are linked at compile-time and copied into the final executable.**

✔ **No external dependencies after linking.**

✔ **Use** `**ar rcs libname.a**` **to create a static library.**

✔ **Always link static libraries after object files (**`**gcc main.c -L. -lmath**`**).**

Would you like a deep dive into **static vs. dynamic linking performance trade-offs**? 🚀

  

  

  

# **🔀 Relocation in Linking: Adjusting Addresses for Execution**

## **🔹 What is Relocation?**

Relocation is the **process of adjusting memory addresses in object files** so that symbols (functions, variables, etc.) point to their correct locations in the final executable or shared library.

**Why is relocation needed?**

1. **Compilers and assemblers generate code starting from address** `**0x0**`, assuming a blank memory space.
2. **When multiple object files are linked together, their sections must be assigned real memory addresses.**
3. The linker **updates addresses in function calls, global variables, and static variables** to reflect their actual locations in memory.

---

## **🔹 When Does Relocation Happen?**

Relocation occurs **twice** in the linking process:

1️⃣ **During Static Linking** (by the linker at compile time)

- The linker assigns absolute addresses and modifies the code accordingly.
- The final executable has no relocation left—it’s **fully resolved**.

2️⃣ **During Dynamic Linking** (by the dynamic linker at runtime)

- Some references remain **unspecified** in the executable.
- The dynamic linker resolves them at **load time or runtime**.

---

## **🔹 How Does Relocation Work?**

The **linker modifies** the following **relocation-sensitive items** in an object file:

- **Function Calls**: `call printf` needs to be changed from an undefined reference to the actual address.
- **Global Variables**: If `int global_var;` is defined in another file, the linker must adjust its address.
- **Static Variables**: Even though `static int x = 10;` is file-scoped, its memory location still needs to be resolved.

The **assembler records relocation entries** in a section called `.rela.text` (for ELF format). These entries tell the linker **which addresses need to be adjusted**.

---

## **🔹 Example: Understanding Relocation in Action**

### **1️⃣ Writing and Compiling a Simple Program**

Let's define a program with a **global variable and a function call**.

### **File:** `**main.c**`

```C
\#include <stdio.h>

int global_var = 10;  // This symbol requires relocation

void foo() {
    printf("Inside foo()\n");
}

int main() {
    foo();  // Function call requires relocation
    return 0;
}
```

Compile without linking:

```Plain
gcc -c main.c -o main.o
```

Now, let’s inspect `main.o` to see relocation entries.

---

### **2️⃣ Viewing Relocation Entries**

To check relocation information in `main.o`, run:

```Plain
readelf -r main.o
```

Example output:

```Plain
Relocation section '.rela.text' at offset 0x400 contains 2 entries:
  Offset      Info         Type           Symbol's Name
  00000013    00030002     R_X86_64_PC32  foo
  0000001a    00050002     R_X86_64_PC32  global_var
```

This tells us:

- The **call to** `**foo()**` **at offset** `**0x13**` needs to be relocated.
- The **reference to** `**global_var**` **at offset** `**0x1a**` needs relocation.
- The **linker will replace these with actual addresses** when creating the final executable.

---

## **🔹 How Relocation Works in Static Linking**

### **1️⃣ Linking Object Files**

Suppose we split our program into two files:

### **File:** `**foo.c**`

```C
\#include <stdio.h>

void foo() {
    printf("Inside foo()\n");
}
```

Compile both files separately:

```Plain
gcc -c main.c -o main.o
gcc -c foo.c -o foo.o
```

Now, link them together:

```Plain
gcc main.o foo.o -o program
```

During linking:

- The linker sees an **undefined reference to** `**foo()**` **in** `**main.o**`.
- It **finds** `**foo()**` **in** `**foo.o**` and **updates the relocation entry** with its actual address.
- Similarly, it assigns an address to `global_var`.

After linking, running:

```Plain
readelf -r program
```

will show **no relocation entries** because everything has been resolved.

✅ **Final executable is fully relocated and independent.**

---

## **🔹 How Relocation Works in Dynamic Linking**

### **1️⃣ Dynamic Symbol Resolution**

Unlike static linking, when we use a **shared library (**`**.so**`**)**, relocation is **not fully resolved at compile time**.

For example, let’s compile `main.c` dynamically:

```Plain
gcc main.c -o program -L. -lfoo
```

If `foo()` is inside `libfoo.so`, the executable still contains **unresolved relocation entries**:

```Plain
readelf -r program
```

```Plain
Relocation section '.rela.plt' at offset 0x400 contains:
  Offset      Info         Type           Symbol's Name
  00000012    00030007     R_X86_64_JUMP_SLOT  foo
```

This means:

- The **function call to** `**foo()**` **is unresolved**.
- The **dynamic linker (**`**ld.so**`**) will resolve** `**foo()**` **at runtime**.

At runtime, `ld.so`:

1. **Finds** `**foo()**` **in** `**libfoo.so**`.
2. **Modifies the jump table (**`**.plt**`**)** so future calls go directly to the correct address.
3. The first call to `foo()` is slow, but subsequent calls are fast.

---

## **🔹 Types of Relocation Entries**

Each **relocation entry** has a **type** that tells the linker how to adjust the address.

|   |   |
|---|---|
|**Type**|**Description**|
|`R_X86_64_32`|Direct 32-bit relocation (absolute addresses).|
|`R_X86_64_PC32`|Relative 32-bit relocation (relative to current position).|
|`R_X86_64_GLOB_DAT`|Used for global variables in shared libraries.|
|`R_X86_64_JUMP_SLOT`|Used for function calls in dynamically linked libraries.|

For example:

- `**call foo**` uses `R_X86_64_PC32` (relative call).
- **Global variables in shared libraries** use `R_X86_64_GLOB_DAT`.

---

## **🔹 What Happens If Relocation Fails?**

### **1️⃣ Undefined Symbols**

If a required symbol **is not found**:

```Plain
ld: main.o: undefined reference to `foo'
```

✅ **Fix:** Ensure `foo.o` or `libfoo.so` is linked.

### **2️⃣ Incorrect Memory Addressing**

If a static library was built for **32-bit** but linked in **64-bit**, relocation can fail.  
✅ **Fix:** Ensure **matching architectures** (`gcc -m32` vs. `gcc -m64`).

---

## **🚀 Key Takeaways**

✔ **Relocation adjusts memory addresses so symbols point to their correct locations.**

✔ **Static linking resolves all relocation at compile time**, producing **independent executables**.

✔ **Dynamic linking defers some relocation until runtime**, using the **dynamic linker (**`**ld.so**`**)**.

✔ **Relocation tables (**`**.rela.text**`**,** `**.rela.plt**`**) tell the linker how to update addresses**.

✔ **Using** `**readelf -r file.o**` **helps inspect relocation entries.**

Would you like a deep dive into **Position-Independent Code (PIC) and how it affects relocation?** 🚀

  

  

# **🔵 Executable Object Files & Loading Them into Memory**

## **🔹 What is an Executable Object File?**

An **executable object file** is a fully linked binary file that can be loaded into memory and executed by the operating system.

📌 **Key Features of an Executable Object File:**

✅ **Fully resolved symbols** (no more relocations needed).

✅ Contains **machine code, initialized & uninitialized data, and metadata**.

✅ Uses a specific format (ELF, PE, Mach-O, etc.), depending on the OS.

✅ Can be directly loaded into memory by the **OS loader**.

---

## **🔹 Object File Formats for Executables**

Different operating systems use different **executable formats**:

|   |   |
|---|---|
|OS|Executable Format|
|**Linux**|**ELF (Executable and Linkable Format)**|
|**Windows**|**PE (Portable Executable)**|
|**MacOS**|**Mach-O (Mach Object Format)**|
|**Old Unix**|**a.out**|

**Modern Linux & Unix systems use ELF**, so let’s focus on that.

---

## **🔹 Structure of an Executable (ELF)**

An **ELF executable** consists of multiple **sections** and **segments**.

### **1️⃣ ELF Header**

- Identifies the file type (`executable`, `shared object`, `relocatable`).
- Contains the **entry point address** where execution starts (`_start` function).
- Defines program headers and section headers.

### **2️⃣ Program Headers (for Loading)**

- **Describes memory layout** for the OS loader.
- Defines **segments**:
    - **Code segment (**`**.text**`**)** → Contains machine instructions.
    - **Data segment (**`**.data**`**,** `**.bss**`**)** → Contains initialized & uninitialized variables.

### **3️⃣ Sections (for Linking)**

- `.text` → Machine code (instructions).
- `.data` → Initialized global variables.
- `.bss` → Uninitialized global variables.
- `.rodata` → Read-only data (e.g., string literals).
- `.symtab` → Symbol table (optional in stripped binaries).
- `.rel.text` → Relocation table (only in relocatable files).
- `.dynamic` → Dynamic linking info (if dynamically linked).

---

## **🔹 How an Executable is Loaded into Memory**

### **1️⃣ Compilation & Linking**

Let’s compile a simple program:

### **File:** `**hello.c**`

```C
\#include <stdio.h>

int main() {
    printf("Hello, world!\n");
    return 0;
}
```

Compile it:

```Plain
gcc hello.c -o hello
```

Now, `hello` is an **executable object file**.

---

### **2️⃣ Loading the Executable into Memory**

When we run `./hello`, the OS loader performs **several steps**:

### **🔵 Step 1: OS Reads the ELF Header**

- The ELF **magic number (**`**0x7F 'ELF'**`**)** confirms it's an ELF binary.
- The OS determines:
    - **Architecture** (x86-64, ARM, etc.).
    - **Endianness** (Little-Endian or Big-Endian).
    - **Type of file** (Executable, Shared Object, etc.).

Check the ELF header with:

```Plain
readelf -h hello
```

Example output:

```Plain
ELF Header:
  Class:                             ELF64
  Type:                              EXEC (Executable file)
  Entry point address:               0x401000
```

📌 The **entry point address (**`**0x401000**`**)** is where execution starts.

---

### **🔵 Step 2: Memory Mapping (Loading Program Headers)**

The OS **maps the program’s segments into memory**:

|   |   |   |
|---|---|---|
|Segment|Virtual Address|Description|
|`.text`|`0x401000`|Code (machine instructions)|
|`.rodata`|`0x404000`|Read-only data (strings, constants)|
|`.data`|`0x406000`|Initialized global variables|
|`.bss`|`0x407000`|Uninitialized global variables|

Check memory mapping:

```Plain
cat /proc/$(pgrep hello)/maps
```

Example:

```Plain
00400000-00401000 r-xp 00000000 08:01 12345  /home/user/hello  (.text)
00600000-00601000 rw-p 00000000 08:01 12345  /home/user/hello  (.data, .bss)
```

---

### **🔵 Step 3: Dynamic Linking (if required)**

If dynamically linked, the loader:

1. Loads **shared libraries (**`**libc.so**`**, etc.)** into memory.
2. Resolves **symbol references**.
3. Updates **GOT/PLT tables** for function calls.

Check if `hello` is dynamically linked:

```Plain
ldd hello
```

Example output:

```Plain
linux-vdso.so.1 =>  (0x00007fffc31f7000)
libc.so.6 => /lib/x86_64-linux-gnu/libc.so.6 (0x00007f3c2ae5a000)
```

📌 **If dynamically linked, the program depends on** `**libc.so.6**`.

---

### **🔵 Step 4: Transfer Control to** `**_start**`

- The **entry point (**`**0x401000**`**)** is executed.
- `_start()` in `crt0.o` (C runtime) sets up:
    - The **stack**.
    - Registers for passing arguments.
    - Calls `main()`.

Disassemble `_start`:

```Plain
objdump -d hello | grep "<_start>"
```

Example:

```Plain
0000000000401000 <_start>:
  401000: 48 83 ec 08     sub $0x8,%rsp
  401004: e8 20 00 00 00  callq 401029 <main>
```

📌 `_start` **sets up the stack and calls** `**main()**`.

---

### **🔵 Step 5: Executing** `**main()**`

- `main()` executes.
- `printf()` may cause a system call (`write` to `stdout`).
- On `return 0;`, execution moves to `**exit()**`.
- `exit()` calls `_exit()` syscall, which tells the OS to terminate the process.

Check system calls:

```Plain
strace ./hello
```

Example output:

```Plain
write(1, "Hello, world!\n", 14)
exit(0)
```

---

## **🔹 Loading a Dynamically Linked vs. Statically Linked Executable**

### **1️⃣ Dynamically Linked (**`**ld.so**` **loads shared libraries)**

```Plain
gcc hello.c -o hello_dynamic -lc
```

- Requires `libc.so.6` at runtime.
- **Smaller binary size** (doesn’t include `printf()`).

### **2️⃣ Statically Linked (**`**no external dependencies**`**)**

```Plain
gcc hello.c -o hello_static -static
```

- **Larger binary size** (includes `printf()` implementation).
- Works **without shared libraries**.

Compare sizes:

```Plain
ls -lh hello_dynamic hello_static
```

Example:

```Plain
-rwxr-xr-x  12K hello_dynamic
-rwxr-xr-x  900K hello_static
```

📌 **Static binaries are much larger** but self-contained.

---

## **🚀 Key Takeaways**

✔ **Executable object files are fully linked binaries ready for execution.**

✔ **ELF format organizes sections (**`**.text**`**,** `**.data**`**,** `**.bss**`**) & program headers.**

✔ **The OS loader maps segments into memory and resolves symbols.**

✔ **Dynamic linking loads shared libraries at runtime.**

✔ **Static executables are self-contained but larger in size.**

✔ **Tools like** `**readelf**`**,** `**objdump**`**,** `**strace**` **help analyze executables.**

Would you like a deep dive into **how system calls interact with the OS kernel during execution?** 🚀

  

  

For the impatient reader, here is a preview of how loading really works: Each program in a Linux  
system runs in the context of a process with its own virtual address space. When the shell runs a program,  
the parent shell process forks a child process that is a duplicate of the parent. The child process invokes  
the loader via the execve system call. The loader deletes the child’s existing virtual memory segments  
and creates a new set of code, data, heap, and stack segments. The new stack and heap segments are  
initialized to zero. The new code and data segments are initialized to the contents of the executable  
file by mapping pages in the virtual address space to page-size chunks of the executable file. Finally,  
the loader jumps to the _start address, which eventually calls the application’s main routine. Aside  
from some header information, there is no copying of data from disk to memory during loading. The  
copying is deferred until the CPU references a mapped virtual page, at which point the operating system  
automatically transfers the page from disk to memory using its paging mechanism.