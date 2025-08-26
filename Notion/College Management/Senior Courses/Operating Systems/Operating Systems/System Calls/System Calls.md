System calls provide an interface to the services made available by an OS. These calls are generally available as functions written in C or C++.

- Programming interface to the services provided by the OS
- typically written in a high-level language (C or C++)
- mostly accessed by programs via a high-level Application Programming Interface or API rather than direct system call use
- Three most common APIs are Win32 API for Windows. POSIX API for POSIX-based systems (including virtually versions of UNIX, Linux, and Mac OS X), and Java API for the JVM.
- Typically, a number is associated with each sys call
    - System-call interface maintains a table indexed according to these numbers
- The system call interface invokes the intended system call in OS kernel and returns status of the system call and any return values
- The caller need know nothing about how the sys call is implemented
    - We just need to obey the API and understand what the OS will do as the result of the call
    - Most details of the OS interface is hidden from the programmer through an API
        - Managed by the run-time support library (set of functions built into libraries included with the compiler) in the language’s RTE or Run-Time Environment

  

Perfect — this is a **core OS concept** and it ties together a lot of the moving parts we’ve been talking about (user mode, kernel mode, interrupts, APIs, scheduling, etc.). Let’s go deep into **System Calls (Section 2.3)**.

---

# 🔹 **System Calls: The OS Interface**

A **system call** is the _programming interface_ between **user applications** and the **operating system kernel**.

- **User programs** → run in **user mode** (restricted privileges).
- **Kernel** → runs in **kernel mode** (full access to hardware and memory).
- System calls are the **only safe way** for user programs to ask the OS to perform privileged tasks.

👉 Without system calls, user programs couldn’t:

- Open or read/write files
- Allocate memory
- Create processes or threads
- Communicate over the network
- Access hardware (keyboard, disk, GPU, etc.)

---

# 🔹 **Step-by-Step Example: File Copy (like UNIX** `**cp**`**)**

Let’s walk through what happens when a program copies a file (`in.txt` → `out.txt`).

### 1. **Get file names**

- **System calls used:**
    - `write()` → print “Enter input file:” to screen
    - `read()` → get user input from keyboard

### 2. **Open files**

- **System calls used:**
    - `open("in.txt")`
    - `open("out.txt", O_CREAT)`
- Errors handled via return codes (`1` if file not found, permission denied, etc.)

### 3. **Copy loop**

- **System calls used repeatedly:**
    - `read(fd_in, buffer, size)`
    - `write(fd_out, buffer, size)`
- Handles cases like _end of file_ or _disk full_.

### 4. **Close and terminate**

- **System calls used:**
    - `close(fd_in)`
    - `close(fd_out)`
    - `exit(status)`

👉 Even this "simple" program makes **dozens of system calls**.

---

# 🔹 **API vs System Calls**

Most programmers **don’t write system calls directly**. Instead, they use an **Application Programming Interface (API)**.

- **API**: Higher-level, user-friendly functions (`fopen()`, `printf()`, `CreateProcess()`).
- **System calls**: Low-level kernel calls (`open()`, `write()`, `fork()`, `exec()`).

Example (Linux):

```C
\#include <unistd.h>
\#include <fcntl.h>

int main() {
    int fd = open("in.txt", O_RDONLY);   // API -> sys_open()
    char buf[100];
    int n = read(fd, buf, 100);          // API -> sys_read()
    write(1, buf, n);                    // API -> sys_write() (stdout = fd=1)
    close(fd);                           // API -> sys_close()
    return 0;                            // exit() system call
}
```

Behind the scenes:

- `open()` in **libc** → triggers a **software interrupt** or special instruction (`syscall`, `int 0x80`, `svc` on ARM).
- Control switches to kernel mode.
- The **system call dispatcher** looks up the function in a **system call table** (indexed by number).
- The kernel executes the function (e.g., `sys_open()` in the VFS layer).
- Kernel returns result → user program continues.

  

![[/image 8.png|image 8.png]]

---

# 🔹 **How Parameters are Passed**

  

System calls need arguments (e.g., file name, buffer address, length).

3 methods are used:

1. **Registers** → If few arguments, load directly into CPU registers.
2. **Memory block/table** → Store arguments in memory, pass pointer.
3. **Stack** → Push parameters, OS pops them off.

Block and stack methods do not limit the number or length of parameters being passed.

👉 Example (Linux x86-64 calling convention for syscalls):

- `rax` → syscall number
- `rdi`, `rsi`, `rdx`, `r10`, `r8`, `r9` → up to 6 arguments
- `syscall` instruction → switches to kernel

---

# 🔹 **Example: Linux** `**read()**`

API:

```C
ssize_t read(int fd, void *buf, size_t count);
```

Steps:

1. User program calls `read(fd, buf, count)`.
2. C library (`glibc`) moves arguments into registers.
3. Loads `rax = 0` (syscall number for `read`).
4. Executes `syscall` instruction.
5. CPU traps into kernel mode.
6. Kernel runs `sys_read(fd, buf, count)`.
7. Copies data from kernel buffer to user buffer.
8. Returns number of bytes read (or `1` on error).

Assembly (x86-64 Linux):

```Assembly
mov rax, 0       ; syscall number for read
mov rdi, fd      ; arg1: file descriptor
mov rsi, buf     ; arg2: buffer pointer
mov rdx, count   ; arg3: number of bytes
syscall          ; enter kernel
```

---

# 🔹 **Why APIs Instead of Direct System Calls?**

1. **Portability**
    - `fopen()` works on Linux, macOS, Windows (with same C code).
    - Direct system calls → OS-specific.
2. **Simplicity**
    - `printf("Hello")` is easier than manually calling `write(1, ...)`.
3. **Safety**
    - The OS may change syscall numbers or add security layers → APIs shield programmers.

---

# 🔹 **Run-Time Environment (RTE)**

Every programming language has its **RTE**, which links your program with system calls.

- C → `libc` (standard library).
- Java → JVM + Java API → system calls.
- Python → CPython interpreter → libc → system calls.

---

# 🔹 **Visualization**

```Plain
User Program
   |
   v
   API (C library, Java API, etc.)
   |
   v
System Call Interface (syscall table, dispatcher)
   |
   v
Kernel Implementation (sys_open, sys_read, sys_write, ...)
   |
   v
Hardware (disk, memory, CPU, I/O devices)
```

---

# 🔹 **Key Takeaways**

- **System calls** = gateway between user space and kernel space.
- **APIs** wrap system calls for portability and convenience.
- **Parameter passing** depends on architecture (registers, stack, tables).
- System calls are _everywhere_ — even `printf("Hello")` makes a syscall.

---

  

# Where are SysCalls defined?

---

### 1. **System Calls = The Contract with the OS**

- Every operating system defines a **system call interface (SCI)**.
- That interface is **fixed and standardized** for that OS and architecture.
- Example: On Linux, `read()`, `write()`, `open()`, `mmap()`, `fork()`, etc. are part of the syscall interface. On Windows, it’s `NtCreateFile`, `NtReadFile`, etc.
- **No application (or programming language) gets to redefine these.** They must call them exactly as the OS specifies.

So yes—every "complex software" out there is sitting _on top_ of those syscalls.

---

### 2. **Where libc Fits**

- In C (Linux world), `libc` is a **thin wrapper** around system calls.
- For example:
    - `printf()` → goes through libc → uses `write()` system call.
    - `malloc()` → may use `mmap()` or `brk()` system calls internally.
- Some libc functions are **pure user-space** (e.g., `strlen()` just scans memory, no syscall). Others **cross into the kernel** (via syscall).

---

### 3. **What Language Are System Calls Written In?**

- System calls themselves are not "written in a high-level language."
- The _interface_ is assembly glue → `syscall` instruction → trap into kernel.
- The **implementation of system calls lives in the OS kernel**, which is mostly written in **C (with some assembly)**.

Example:

```C
// Linux kernel implementation of sys_write
SYSCALL_DEFINE3(write, unsigned int, fd, const char __user *, buf, size_t, count) {
    return ksys_write(fd, buf, count);
}
```

That’s kernel C code—not accessible directly by user programs.

---

### 4. **Programming Languages and Syscalls**

- High-level languages like Python, Java, Rust, Go—none of them **invent their own syscalls**.
- Instead:
    - Python’s `open("file.txt")` → implemented in C → calls libc’s `open()` → syscall.
    - Java’s `FileInputStream` → JVM native methods → libc → syscall.
    - Rust’s `std::fs::File` → implemented in Rust → but under the hood, calls libc → syscall.

So yes—even the **creators of programming languages** must obey the system call interface. They can only _wrap it differently_.

---

### 5. **Can They Modify Syscalls?**

- **No, not without modifying the kernel itself.**
- If you want a new syscall → you must patch and recompile the kernel.
- Otherwise, you just build higher abstractions on top of the fixed syscall set.

---

✅ **In short**:

All modern software—even entire languages—rests on the foundation of system calls. They don’t change the OS syscall interface; they only **wrap**, **abstract**, or **extend** it in user space. The syscall interface is like the "laws of physics" in your computer—you can build rockets, cars, and planes, but you can’t rewrite gravity unless you rewrite the universe (kernel).

---

Do you want me to show you a **step-by-step code path** of how something like `printf("hello")` goes all the way down into an actual syscall in assembly and kernel C?

  

  

  

Perfect — this section you pasted is **one of the most important parts of OS design** because it explains _what system calls are, how they’re grouped, and why they exist_.

Let’s go through it in a **structured lecture format**, expanding concepts, linking them to real-world examples (Linux, Windows, etc.), and connecting them to deeper OS principles.

---

# 🔹 Lecture: Types of System Calls (Operating System Interface)

---

## 2. Categories of System Calls

The text identifies **six major categories** of system calls. Let’s go one by one:

---

### 2.1 Process Control

Processes are the "active entities" in an OS. System calls here deal with:

- **Process Creation & Termination**
    - `fork()` (UNIX/Linux): Duplicates a process.
    - `exec()` (UNIX/Linux): Replaces current process with a new program.
    - `CreateProcess()` (Windows): Creates a new process in one step.
    - `exit()` (UNIX/Linux), `ExitProcess()` (Windows): Terminates a process.
- **Loading & Execution**
    - Loading a binary into memory (`exec` in UNIX).
    - In Windows, the loader is part of `CreateProcess()`.
- **Waiting & Signaling**
    - A parent process may `wait()` for a child to finish.
    - Processes can signal each other using events or signals (`kill(pid, SIGTERM)`).
- **Attributes**
    - Get/set process priority (`nice()` in Linux).
    - Adjust scheduling, execution time, memory usage.
- **Memory Management**
    - `malloc()` in C library eventually calls system calls like `brk()` or `mmap()`.
    - dumb memory if error
- Debugger for determining bugs, single step execution
- Locks for managing access to shared data between processes

**Example**:

When you type `ls` in a UNIX shell:

1. Shell calls `fork()` → creates a child process.
2. Child calls `exec("ls")` → replaces itself with the `ls` program.
3. Parent calls `wait()` → waits for child to finish.
4. Child eventually calls `exit()`.

👉 This illustrates the tight relationship between **system calls and process lifecycle**.

---

### 2.2 File Management

Files are the most common abstraction in an OS. System calls handle:

- **File creation & deletion**: `open()`, `create()`, `unlink()` in UNIX; `CreateFile()` in Windows.
- **Reading/Writing**: `read()`, `write()` (UNIX/Linux); `ReadFile()`, `WriteFile()` (Windows).
- **Repositioning**: `lseek()` (move file pointer).
- **Attributes**: `stat()` to get metadata; `chmod()` to change permissions.

**Note**: In UNIX, _everything is a file_ — even devices. This unification means the same calls (`read`, `write`) work on:

- Regular files
- Terminals
- Network sockets
- Devices

👉 This consistency is why UNIX became so elegant.

---

### 2.3 Device Management

- Processes need access to hardware (disk, keyboard, GPU, etc.).
- System calls:
    - `ioctl()` (UNIX): control operations on devices.
    - `request device` / `release device`: OS ensures exclusive access.
    - `read/write` (same as files).
- **Why?** Prevents two processes from clashing (e.g., two programs writing to the same printer).

**Design insight**: Many OSs treat devices as files (device special files in `/dev` on UNIX).

---

### 2.4 Information Maintenance

System calls that query or set system information:

- **Time & Date**: `time()`, `gettimeofday()`.
- **System Info**: `uname()` in UNIX gives OS version, hostname, etc.
- **Process Info**: `getpid()` returns current process ID.
- **Debugging & Profiling**: Some calls dump memory, or allow tracing (`ptrace()` in UNIX).

👉 Important for monitoring, debugging, and system administration.

---

### 2.5 Communications

Processes often need to exchange data. Two main models:

### a) Message Passing

- Processes send messages (like letters).
- May use pipes, sockets, message queues.
- Safer (no shared memory conflicts), good for distributed systems.
- Examples:
    - `pipe()` (UNIX).
    - `send()`, `recv()` for sockets.
    - `CreatePipe()` in Windows.

### b) Shared Memory

- Two processes map the same memory region and communicate via reads/writes.
- Faster (no kernel mediation after setup).
- Requires synchronization (locks, semaphores).
- Calls: `shmget()`, `mmap()`, `shmat()` in UNIX/Linux.

👉 Most modern OSs support both.

Example: Databases use **shared memory** (for speed), whereas web servers often use **sockets** (for safety and scalability).

---

### 2.6 Protection

Since multiple users/programs share the system, protection ensures **who can do what**:

- **File permissions**: `chmod()`, `chown()` in UNIX; `SetFileSecurity()` in Windows.
- **Access Control**: Which users can access certain devices or processes.
- **Example**: On UNIX, only `root` can bind to ports < 1024.

Protection extends to:

- Memory (one process shouldn’t read another’s private memory).
- Devices (prevent conflicts).
- Networks (firewall rules, etc.).

👉 This area grows into **Security**, which deals with external threats.

---

## 3. Example: The C Library and System Calls

Applications rarely call system calls directly. Instead:

- They call functions from the **C standard library (**`**libc**`**)**.
- `printf("Hello")` → eventually calls `write()` system call.
- This indirection gives portability: the same `printf()` works on UNIX, Linux, Windows (with different backends).

👉 **Key takeaway**: Application → C library → System call → Kernel → Hardware.

---

## 4. Comparison: UNIX vs Windows System Calls

The textbook table shows equivalences:

|Category|UNIX/Linux Example|Windows Example|
|---|---|---|
|Process Control|`fork()`, `exec()`|`CreateProcess()`|
|File Management|`open()`, `read()`|`CreateFile()`, `ReadFile()`|
|Device Management|`ioctl()`|`SetConsoleMode()`|
|Info Maintenance|`getpid()`, `time()`|`GetCurrentProcessID()`|
|Communication|`pipe()`, `mmap()`|`CreatePipe()`, `MapViewOfFile()`|
|Protection|`chmod()`, `chown()`|`SetFileSecurity()`|

👉 UNIX tends to have many **small, orthogonal system calls**.

👉 Windows tends to have **fewer, more powerful system calls** (often doing multiple steps).

---

## 5. Why This Matters

- System calls define the **boundary** between user space and kernel space.
- They reflect the **design philosophy** of the OS:
    - UNIX: simplicity, “everything is a file”.
    - Windows: integrated API, closer to GUI needs.
- **Security, performance, and usability** all depend on how system calls are designed.

---

✅ **Summary:**

System calls are the glue between programs and the OS kernel. They fall into six broad categories: **process control, file management, device management, information maintenance, communications, and protection**. All modern programs—even GUI apps—ultimately rely on these low-level calls, usually mediated through libraries like `libc`. Understanding them gives deep insight into how operating systems provide services while maintaining protection, efficiency, and user convenience.

---

  

  

# System Services

They provide a convenient environment for program development and execution

Perfect question 👍 — this is where we move one level _above_ system calls into **system services (a.k.a. system programs or utilities)**.

System calls are the raw, low-level API between user programs and the kernel, but system services are **the higher-level tools that make an OS usable**. They bridge the gap between _kernel internals_ and _end-user experience_.

Let’s go through this in an **in-depth lecture style**, linking theory → practice → real-world examples.

---

# 🔹 Lecture: System Services (System Programs / Utilities)

## 1. The Layered Model Refresher

Recall the OS stack:

```Plain
+---------------------------+
|   Application Programs    |   (e.g., browsers, games, editors)
+---------------------------+
|     System Services       |   (compilers, shells, file utilities, daemons)
+---------------------------+
|   Operating System Kernel |   (system calls, process mgmt, memory, I/O)
+---------------------------+
|          Hardware         |
+---------------------------+
```

- **System Calls**: primitive operations (e.g., `open()`, `fork()`, `read()`).
- **System Services**: user-facing programs/utilities that wrap those primitives into convenient, higher-level tools.
- **Applications**: use both system calls (via libraries) and system services.

👉 Example:

To list files in a directory:

- **System call**: `getdents()` (Linux).
- **System service**: `/bin/ls` program calls `getdents()` internally.
- **Application**: you type `ls` in a shell, or a GUI file explorer calls it under the hood.

---

## 2. Categories of System Services

### 2.1 File Management

- Purpose: Provide human-friendly utilities to manipulate files/directories.
- Examples:
    - UNIX/Linux: `ls`, `cp`, `mv`, `rm`, `cat`, `more`.
    - Windows: Explorer GUI, `copy`, `del` in CMD.
- Under the hood: Call system calls like `open()`, `read()`, `write()`, `unlink()`.

**Practical note**:

The `cp` command in UNIX is nothing more than:

1. Open source file (`open()`).
2. Open destination file (`creat()`).
3. Read blocks (`read()`).
4. Write blocks (`write()`).
5. Close files (`close()`).

So file management services = **wrappers** around basic syscalls.

---

### 2.2 Status Information

- Provide info about the system state.
- **Simple**: `date`, `df` (disk free), `who`, `uptime`.
- **Complex**: performance monitors, debuggers, profiling tools.
- **Windows**: Task Manager, Event Viewer, `systeminfo` command.
- Often involves querying `/proc` (Linux virtual filesystem exposing kernel info) or registry (Windows).

**Example**:

`top` command = repeatedly reading `/proc/stat`, `/proc/meminfo` → formatting into live CPU/memory usage.

---

### 2.3 File Modification

- Editors, search utilities, formatters.
- Examples:
    - Editors: `vi`, `nano`, `emacs`, Notepad (Windows).
    - Search: `grep`, `findstr`.
    - Transform: `awk`, `sed`.
- Critical because they allow users to **create and modify persistent data**.

---

### 2.4 Programming-Language Support

- OS often bundles **toolchains** so you can build software.
- **Assemblers** (`as`, MASM), **Compilers** (`gcc`, `clang`, `javac`), **Interpreters** (Python, Perl).
- Debuggers (`gdb`, WinDbg).
- Without this, OS is just an execution platform, not a development environment.

👉 Historically:

- UNIX came bundled with C compiler (self-hosting).
- Windows ships with no compiler by default (developers install Visual Studio).

---

### 2.5 Program Loading & Execution

- Tools for preparing programs for execution.
- **Loaders**: put program into memory (`ld.so` in Linux, PE loader in Windows).
- **Linkers**: combine object files into executables (`ld`, `link.exe`).
- **Debuggers**: run programs under supervision, step by step.

👉 Example workflow:

`gcc hello.c -o hello` → compiler + linker produce `hello` → loader maps it into memory → OS executes it with `execve()`.

---

### 2.6 Communications

- Mechanisms for inter-process and inter-user communication.
- Examples:
    - Local: `mail`, `write`, `wall` (send message to all users).
    - Network: `ssh`, `ftp`, `ping`, web browsers.
- GUI perspective: Browsers, chat clients.
- Underneath: rely on socket system calls (`socket()`, `connect()`, `send()`, `recv()`).

---

### 2.7 Background Services (Daemons / Services)

- Programs started at boot or on demand, running silently in the background.
- UNIX/Linux: **daemons** (traditionally named with a `d`: `sshd`, `httpd`, `cron`, `syslogd`).
- Windows: **services** (e.g., Print Spooler, Windows Update).
- Examples:
    - Scheduler (`cron` in UNIX, Task Scheduler in Windows).
    - Logging service.
    - Network listeners.
- Essential for system operation but invisible to the user.

👉 Think of daemons as "always-running assistants" that handle recurring or event-driven tasks.

---

## 3. Distinction: System Calls vs System Services

- **System Calls**: Low-level kernel entry points, always there (defined by OS ABI).
- **System Services**: Programs/tools built on top of system calls, can vary across OS distributions.

Example:

Linux without `ls` still has system calls to read directories (`getdents()`) — you could write your own `ls`.

But most users interact with `ls`, not the syscall.

---

## 4. User Perspective: Why System Services Matter

- Users don’t "see" system calls.
- They see **services, shells, and GUIs**.
- macOS Finder, Windows Explorer, GNOME/Nautilus: all system services layered over syscalls.

👉 This is why the same hardware (say, dual-boot Windows/macOS) feels _completely different_:

Both use the same CPU, memory, and syscalls at a low level, but **system services + UI + bundled apps** define the user experience.

---

## 5. Modern Example: Linux Boot → Services

1. Kernel boots, mounts root filesystem.
2. `systemd` starts as PID 1 (service manager).
3. `systemd` launches:
    - `sshd` (remote login daemon),
    - `cupsd` (print daemon),
    - `cron` (job scheduler),
    - networking daemons.
4. User logs in → gets access to shells (`bash`, `zsh`) or GUI (GNOME).
5. User uses system services (`ls`, `vim`, `gcc`) to interact with the machine.

---

## ✅ Summary

- **System Calls** = low-level kernel API.
- **System Services** = higher-level utilities, making the system usable for humans.
- Categories: file management, status info, file modification, programming support, program execution, communications, background services.
- Daemons/services extend the OS’s power silently in the background.
- Ultimately: system services define the _user’s view_ of the OS, while system calls define the _programmer’s view_.

---

Would you like me to go further and show you **a code example** (e.g., how a system service like `ls` internally uses system calls like `openat()` + `getdents64()`), or would you prefer a **visual diagram** of the OS layer with examples of services?

  

  

# Linkers and Loaders

Usually, a program resides on disk as a binary executable file—like a.out, prog.exe. To run on a CPU, the program must be brought into memory and placed in the context of a process.

- Source files are compiled into object files that are designed to be loaded into any physical memory location, a format known as a relocatable object file.

  

---

# 🔹 1. The High-Level Pipeline

When you write a program like this:

```C
\#include <stdio.h>

int main() {
    printf("Hello, World!\n");
    return 0;
}
```

And you compile with:

```Bash
gcc -o hello hello.c
```

Behind the scenes, a **four-stage pipeline** happens:

1. **Compilation → Object files (.o)**
    - Each source file is translated into **machine code fragments**, but they are not yet fully placed in memory.
    - These are called **relocatable object files** (they can be loaded anywhere in memory).
2. **Linking → Executable file (a.out, hello, .exe, etc.)**
    - Combines object files and libraries into one unified binary.
    - Resolves **symbols** (function calls, global variables).
    - Produces an **executable file format** (ELF on Linux, PE on Windows, Mach-O on macOS).
3. **Loading → Placing program into memory**
    - When you type `./hello`, the **loader** (part of the OS kernel) loads your program into memory.
    - Sets up **stack, heap, code, data segments**.
    - Jumps to the **entry point** (usually `_start` in libc, not directly `main`!).
4. **Execution → Process running on CPU**
    - The CPU starts executing your code instructions.
    - Your `main()` runs after some runtime setup by libc.

---

# 🔹 2. Step 1: Compilation → Object Files

When you run:

```Bash
gcc -c main.c   # produces main.o
```

The compiler produces **machine code** but doesn’t yet know where functions/variables will live in memory.

👉 Example: Suppose `main.c` has:

```C
int main() {
    return add(2, 3);   // function declared elsewhere
}
```

The compiler doesn’t know where `add()` is located yet. So in `main.o`, it creates:

- **Machine code for** `**main**`
- A **symbol table entry** saying: "This program calls `add`, but I don’t know where it is. Someone must resolve this later."

This is why object files are called **relocatable object files**: they can be combined later by the linker.

On Linux, object files use the **ELF Relocatable format**.

Check it with:

```Bash
file main.o
# output: ELF 64-bit LSB relocatable, ...
```

---

# 🔹 3. Step 2: Linking → Executable File

The **linker** (e.g., `ld`, invoked by `gcc`) takes all `.o` files + libraries and combines them.

```Bash
gcc -o program main.o add.o -lm
```

### What happens:

1. **Symbol Resolution**
    - The linker matches **unresolved symbols** (`add` in `main.o`) with their definitions (`add` in `add.o` or `libm.so` for math functions).
2. **Relocation**
    
    - Once all symbols are resolved, the linker **assigns absolute addresses**.
    - For example, it decides:
        - `main` starts at 0x400580
        - `add` starts at 0x400600
        - Global variable `counter` at 0x601020
    
    The linker then **patches** all call/jump instructions to point to the correct addresses.
    
3. **Produce Executable File**
    - The result is an **executable binary** (ELF Executable on Linux).
    - Contains:
        - `.text` (machine code)
        - `.data` (initialized global variables)
        - `.bss` (uninitialized globals)
        - **Entry point** (first instruction to run).

Check with:

```Bash
file program
# output: ELF 64-bit LSB executable, ...
```

---

# 🔹 4. Step 3: Loading → Process in Memory

When you type:

```Bash
./program
```

The following happens:

1. **The shell calls fork()**
    - Creates a new process.
2. **The child process calls execve("program")**
    - Replaces the child’s memory image with the new program.
3. **The loader (part of the kernel) runs**:
    - Reads the ELF file header.
    - Maps sections of the file into memory (`mmap`).
    - Sets up stack, heap, shared libraries.
    - Places arguments (`argv`, `envp`) on the stack.
    - Jumps to the **entry point** (usually `_start`).

👉 Note: `_start` is a small assembly function in libc that:

- Calls `__libc_start_main()`.
- Sets up runtime environment.
- Calls your `main()`.
- Calls `exit()` when `main` returns.

---

# 🔹 5. Step 4: Dynamic Linking

Not all code is linked at compile time. To save memory, OSes support **dynamic linking**:

- **Static linking**: library code is copied into your binary.
    - Pros: portable, no external dependencies.
    - Cons: big binary, duplicates across processes.
- **Dynamic linking**: library code (e.g., `libc.so`) is stored once in memory, shared by all processes.
    - The executable just contains **relocation stubs** that say:
        
        “When I call `printf`, look it up in libc at runtime.”
        

On Linux, shared libraries are `.so` (shared object). On Windows: `.dll`.

When you run the program:

- The **dynamic linker/loader (**`**ld.so**`**)** runs.
- It resolves symbols (like `printf`) by loading the right `.so` files into memory.
- Applies relocations so that calls point to the right place.

👉 Example: Run with library tracing:

```Bash
LD_DEBUG=libs ./program
```

You’ll see which shared libraries are loaded.

---

# 🔹 6. File Formats

Each OS has its own executable file format:

- **ELF (Executable and Linkable Format)** → Linux/Unix
- **PE (Portable Executable)** → Windows
- **Mach-O** → macOS

Each format defines:

- How headers, code, data, and symbols are stored.
- Where the **entry point** is.
- How relocations and dynamic linking work.

---

# 🔹 7. Example Walkthrough

Let’s trace a simple program:

```C
\#include <math.h>
\#include <stdio.h>

int main() {
    printf("%f\n", sqrt(16.0));
    return 0;
}
```

Compile:

```Bash
gcc -o test test.c -lm
```

Steps:

1. `gcc` compiles → `test.o`
    - Symbol table: `main`, unresolved `printf`, unresolved `sqrt`.
2. Linker links → `test`
    - Adds stubs for `printf` (from libc.so) and `sqrt` (from libm.so).
    - Produces ELF executable.
3. Run `./test`
    - Shell forks + execs.
    - Kernel loader loads ELF.
    - `ld.so` maps libc.so and libm.so into memory.
    - Relocations applied.
    - CPU jumps to `_start`, which calls `main`.
4. Output: `4.000000`

---

✅ So in summary:

- **Compiler** → generates **object code** with unresolved references.
- **Linker** → resolves symbols, relocates addresses, makes executable.
- **Loader** → maps binary into memory, sets up runtime.
- **Dynamic linker** → loads shared libraries, resolves functions.

---

Would you like me to also show you a **real example with** `**readelf**` **and** `**objdump**` **commands** so you can see inside object files, executables, and where the entry point is? That way, you’ll see the actual symbols, relocation entries, and memory layout.

![[/image 1 4.png|image 1 4.png]]

  

# Why Applications are OS-Specific

Perfect — this is one of the most important sections in understanding **why software is OS-dependent**. Let’s break down **2.6 Why Applications Are Operating-System Specific** in depth. I’ll expand on each idea, clarify with examples, and connect it back to the system call + linker/loader discussion you asked about earlier.

---

# 🔹 Why Applications Are Operating-System Specific

At first glance, it might seem like once you compile a program into binary, you should be able to run it anywhere. After all, it’s just “machine code,” right?

But here’s the problem:

1. **Machine instructions differ per CPU architecture (x86 vs ARM vs RISC-V, etc.).**
2. **Binary executable formats differ per OS (ELF for Linux, PE for Windows, Mach-O for macOS).**
3. **System calls differ across OSes (different numbers, calling conventions, semantics).**
4. **APIs and libraries differ (Windows GUI APIs vs Linux/X11 vs iOS/UIKit).**

Because of these mismatches, **a compiled binary is tied to both the OS and the CPU architecture.**

---

## 1. The Role of System Calls

Every OS exposes a set of **system calls**. These are the “entry points” into the kernel.

- Linux has ~400+ system calls (`open`, `read`, `write`, `fork`, `execve`, etc.).
- Windows has its own set (`CreateFileW`, `ReadFile`, `WriteFile`, `CreateProcess`, etc.).
- macOS has yet another set.

👉 Even if two OSes have “similar” syscalls (e.g., open a file), the **interfaces differ**:

- Linux `open()` takes a filename and flags.
- Windows `CreateFileW()` takes a filename, desired access flags, security attributes, share modes, etc.

So a binary compiled for Linux will call Linux syscall numbers (via `int 0x80`, `syscall`, or `vsyscall`). If you try to run that binary on Windows, Windows won’t even know what “syscall \#2” means.

---

## 2. Binary Formats: ELF, PE, Mach-O

When you compile a program, the **executable is wrapped in a binary format** that the loader understands.

- **Linux/Unix** → ELF (Executable and Linkable Format).
- **Windows** → PE (Portable Executable).
- **macOS** → Mach-O.

These formats define:

- The **header structure** (magic number, entry point, section tables).
- The **memory layout** (where `.text`, `.data`, `.bss`, dynamic linking info go).
- How **relocation** works.
- Where to find the program’s **entry point** (the first instruction to run).

If you give Linux a Windows PE file (`.exe`), it won’t know how to parse it. That’s why something like **Wine** exists—it reimplements Windows’ loader, system calls, and APIs on Linux.

---

## 3. CPU Instruction Set Differences

Even if we solved the OS syscall and binary format problem, we still have **CPU differences**.

- A binary compiled for **x86 (Intel/AMD)** will not run on **ARM (phones, Raspberry Pi)** because the CPU doesn’t recognize the instructions.
- That’s why software must either:
    - Be recompiled for each architecture (cross-compilation), or
    - Use a **virtual machine or interpreter** that hides CPU differences (e.g., JVM for Java, Python interpreter, .NET CLR).

This is also why Android apps (ARM) can’t run directly on Windows (x86) without emulation.

---

## 4. Approaches to Cross-Platform Applications

The text mentions **three strategies** to make applications portable:

### (A) Interpreted Languages (Python, Ruby, JavaScript)

- You write **source code** in a high-level language.
- An **interpreter** (written in C, compiled for each OS/CPU) runs it. It reads each line of the source program.
- The interpreter itself makes the syscalls appropriate for the OS. It executes equivalent instructions on the native instruction set, and calls native operating system calls.
- Example: `python script.py` runs on Linux, Windows, macOS as long as you have Python installed.

**Pros:** Portable.

**Cons:** Slower, limited OS feature access.

---

### (B) Virtual Machines + RTEs (Java, .NET, WebAssembly)

- You compile into **bytecode** (e.g., Java `.class` files, .NET IL, WebAssembly `.wasm`).
- The **runtime environment (RTE)** runs the bytecode on any system with a VM.
- The RTE itself is compiled per OS/CPU, and knows how to call syscalls.
- Example: A Java program can run on Linux, Windows, or macOS because the JVM abstracts the OS differences.

**Pros:** Faster than pure interpretation, very portable.

**Cons:** Extra overhead, dependent on RTE availability.

---

### (C) Portable APIs + Porting (C/C++ with POSIX, Qt, etc.)

- You write in a compiled language (C, C++, Rust, Go).
- But you stick to a **portable API** (e.g., POSIX for Unix-like systems).
- Your code is **recompiled** for each OS, linking against the right libraries.
- Example: Firefox, Chrome, VLC — the same source codebase is compiled separately for each OS.

**Pros:** Best performance, full native features.

**Cons:** Requires porting effort, testing, and multiple builds.

---

## 5. The Role of the ABI

At the **binary level**, we have the **ABI (Application Binary Interface)**. This is at the architecture level. It is used to define how different components of binary code can interface for a given OS on a given architecture. However, because a particular ABI is defined for a certain OS running on a given architecture, ABIs do little to provide cross-platform compatibility.

An ABI defines:

- **Calling conventions** (how functions pass arguments: stack vs registers).
- **Data type sizes** (`int` = 4 bytes vs 8 bytes).
- **Struct alignment rules**.
- **Syscall invocation method** (`int 0x80` vs `syscall` instruction).
- **Endianness** (big vs little).
- **Binary format (ELF, PE, Mach-O)**.

👉 Even if two systems have the **same CPU** (say, x86-64), a Linux binary won’t run on Windows because the **ABI differs**.

---

## 6. Real-World Example: Firefox

Firefox runs on **Windows, macOS, Linux, Android, iOS**.

To achieve this:

- **Shared codebase** in C++ and Rust.
- **Abstracted APIs** (POSIX-like for Linux, Win32 for Windows, Cocoa for macOS, etc.).
- **Different builds per platform** (different compilers, linkers, loaders).
- **Platform-specific code** (Windows UI uses Win32 APIs, Linux uses GTK, macOS uses Cocoa).

It’s not “one binary for all OSes.” It’s **multiple binaries from one source codebase.**

---

# 🔹 Summary

Applications are OS-specific because of:

1. **Different system calls.**
2. **Different binary formats.**
3. **Different CPU instruction sets.**
4. **Different ABIs.**

Cross-platform software exists, but only because of **interpreters, virtual machines, or portable APIs**.

Even then, **developers usually compile separate binaries per OS/CPU**, not one universal binary.

That’s why making something like **Wine (Windows on Linux)** or **Proton (Windows games on Steam/Linux)** is so difficult: they must reimplement or translate **entire ABIs, syscalls, and binary formats**.

---

👉 Question for you: Do you want me to also show you **a practical demo** (with code + commands) of how an ELF binary on Linux contains syscall numbers and why it won’t run on Windows? That way, you’ll see the OS-specific part in action.