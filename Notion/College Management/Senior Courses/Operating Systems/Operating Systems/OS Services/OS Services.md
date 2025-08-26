An OS provides the environment within which programs are executed. It makes certain services available to programs and to the users of those programs

![[/image 7.png|image 7.png]]

  

One set of OS services provides functions that are helpful to the user:

  

---

# **Lecture: Operating System Services**

An **Operating System (OS)** is essentially the **intermediary** between the **hardware** of a computer system and the **applications/users** who want to use that hardware.

It provides:

1. An **execution environment** for programs.
2. A set of **services** that make using the computer system easier, safer, and more efficient.

Broadly, OS services fall into two categories:

- **User-facing services**: Make life easier for programmers and users.
- **System-facing services**: Ensure efficiency, fairness, protection, and stability of the system itself.

---

## **1. Services Helpful to Users**

### **(a) User Interface (UI)**

- This is the **point of interaction** between the user and the operating system.
- Common forms:
    - **Graphical User Interface (GUI):** Desktop environments (Windows, macOS, GNOME, KDE). Uses windows, icons, menus, mouse, touchscreen.
    - **Command-Line Interface (CLI):** Text-based commands (Linux shell, PowerShell).
    - **Touch Interface:** Common in smartphones and tablets. Users interact via gestures, taps, swipes.

👉 Example:

- In Linux, you can interact via a GUI (GNOME desktop) _or_ via CLI (`bash` shell).
- Mobile OS like Android primarily uses **touchscreen** UIs.

---

### **(b) Program Execution**

- The OS must be able to:
    - **Load a program into memory** from storage.
    - **Start execution** of that program.
    - Handle program termination (normal exit or abnormal due to error).

👉 Example:

When you double-click on `chrome.exe` in Windows:

- The **loader** brings Chrome into RAM.
- The OS sets up execution context (stack, heap, registers).
- Chrome executes until you quit or it crashes.

---

### **(c) I/O Operations**

- Programs need input/output. Examples:
    - Reading a file.
    - Writing to the screen.
    - Sending data over a network.
- **Users cannot directly control devices** (unsafe and inefficient).
- Instead, the OS provides **system calls** for I/O.

👉 Example in C (Linux):

```C
int fd = open("notes.txt", O_RDONLY);   // open file
read(fd, buffer, 100);                  // read from file
close(fd);                              // close file
```

Here, `open`, `read`, and `close` are **system calls** → OS handles the actual disk operations.

---

### **(d) File-System Manipulation**

- Programs frequently need to **create, delete, read, write, search, and list** files and directories.
- OS provides a **file system abstraction** → you don’t deal with raw disk sectors.
- Includes **permissions management** (who can access/modify files).

👉 Example:

- In Linux: `ls -l` shows file permissions (`rwxr-xr--`).
- In Windows: File properties → Security tab (read/write/execute access).

---

### **(e) Communications**

- Processes often need to communicate, either:
    - **Same system:** Inter-process communication (IPC).
    - **Different systems:** Networking.
- Two major communication models:
    - **Shared Memory:** Multiple processes access the same memory region. Faster, but needs synchronization.
    - **Message Passing:** Processes send/receive structured messages. Simpler, works across distributed systems.

👉 Example:

- Shared memory in Linux: `shmget`, `shmat`.
- Message passing: `MPI` in high-performance computing clusters.
- Networking: `send()`, `recv()` sockets API.

---

### **(f) Error Detection**

- Errors may occur at any layer:
    - **Hardware errors:** Memory failure, CPU errors, disk parity error.
    - **I/O errors:** Printer out of paper, disk read error, network cable unplugged.
    - **Software errors:** Invalid memory access, divide by zero.
- OS must **detect and handle** these errors:
    - Kill faulty process.
    - Return error codes.
    - Log event.

👉 Example:

In Linux, if you access invalid memory, you get a **segmentation fault (SIGSEGV)**.

---

## **2. Services for Efficient System Operation**

These aren’t directly about helping the user — they’re about **managing resources and keeping the system stable**.

---

### **(a) Resource Allocation**

- Many processes run simultaneously. OS must allocate:
    - **CPU** (via scheduling algorithms: round robin, priority scheduling, etc.).
    - **Memory** (via memory management units).
    - **I/O devices** (via device queues).

👉 Example:

- If 10 processes want to print, OS schedules them fairly.
- If multiple processes compete for CPU → OS decides who runs next.

---

### **(b) Logging (Accounting)**

- OS keeps track of **which user/program used which resources**.
- Used for:
    - Billing (in mainframes, cloud systems).
    - Performance tuning.
    - Debugging system performance.

👉 Example:

- Linux has `/var/log/` containing logs.
- Cloud providers (AWS, GCP) bill users based on CPU, RAM, bandwidth usage.

---

### **(c) Protection and Security**

- **Protection:** Ensuring processes cannot interfere with one another or the OS. (E.g., memory protection → process cannot access another’s memory).
- **Security:** Defending against malicious outsiders.
    - User authentication (passwords, biometrics).
    - Access control (who can open a file?).
    - Network security (firewalls, intrusion detection).

👉 Example:

- In Linux: File permissions, `sudo`.
- In Windows: UAC (User Account Control), Defender antivirus.

---

## **3. Big Picture (Figure 2.1 Recap)**

- **User and system programs** interact with the **OS services**.
- **OS services** sit above the **hardware** layer.
- Services split between:
    - **User-oriented (UI, program execution, I/O, file system, communication, error detection).**
    - **System-oriented (resource allocation, logging, protection & security).**

---

## **Summary**

- The OS provides **services** that abstract raw hardware complexity and provide a safe, efficient execution environment.
- Services are grouped into two categories:
    - **User services** (make life easier for programmers/users).
    - **System services** (ensure system efficiency, fairness, and security).
- These services rely heavily on **system calls** as the interface between user programs and the OS kernel.

---

  

  

  

---

# 📖 Lecture: User and Operating-System Interfaces

The **user interface (UI)** is how humans interact with the operating system. Without a UI, an operating system would just be a kernel running instructions invisibly. There are **three fundamental approaches** to OS-user interaction:

1. **Command-Line Interface (CLI)**
2. **Graphical User Interface (GUI)**
3. **Touch-Screen Interface**

Finally, there is also the issue of **Choice of Interface**, which depends on user preference and system role.

---

## 1. Command-Line Interface (CLI)

### 📌 Definition

A **command interpreter** (often called a **shell**) allows the user to type commands directly into the system. The OS then executes those commands.

- On **UNIX/Linux**, you’ll see shells like:
    - **Bash (Bourne-Again Shell)**
    - **C Shell (csh)**
    - **Korn Shell (ksh)**
- On **Windows**, the traditional CLI is **Command Prompt** (`cmd.exe`), while more modern environments use **PowerShell**.

### 📌 How it Works

- When you log in, the OS starts a shell as your first process.
- The shell waits for user input, parses the command, and either:
    1. Executes it **internally** (built-in commands).
    2. Loads a **system program** (like `rm` or `ls`) into memory and runs it.

### Example:

```Bash
rm file.txt
```

- The shell finds `/bin/rm` (a system program).
- It **loads it into memory** and executes it with the argument `file.txt`.
- The shell itself doesn’t “know” how to delete a file; it simply invokes the external program.

This makes UNIX shells very **modular**: new commands can be added without modifying the shell itself.

### 📌 Built-in vs System Program Commands

- **Built-in (internal):**
    - Example: `cd` (change directory) → implemented _inside_ the shell, not as an external program.
- **System program (external):**
    - Example: `rm file.txt` → calls `/bin/rm`.

### 📌 Why CLI Matters

- **Power users and sysadmins** prefer CLI for speed, flexibility, and automation.
- CLI allows **scripting**: repetitive tasks can be saved in a file and executed as a "script."

### Example Shell Script:

```Bash
#!/bin/bash
# backup.sh - A simple backup script
tar -czf backup.tar.gz ~/Documents ~/Pictures
echo "Backup completed!"
```

Running this script saves you from typing the same long command every time.

---

## 2. Graphical User Interface (GUI)

### 📌 Definition

A **GUI** replaces textual input with **icons, windows, menus, and pointers**. Users interact with the OS through a **desktop metaphor** (files as icons, drag-and-drop, etc.).

- Introduced at **Xerox PARC (1973)** on the Xerox Alto.
- Became mainstream with:
    - **Apple Macintosh (1984)**
    - **Microsoft Windows (1985 onward)**

### 📌 Features

- Uses a **mouse or trackpad** for pointing and selecting.
- **Windows, menus, and icons** represent programs and files.
- Easy for beginners and casual users.

### 📌 Evolution Examples

- **Mac OS** → started GUI-only, later added CLI with macOS (UNIX kernel + Aqua GUI).
- **Windows** → originally GUI layer on MS-DOS, now a fully integrated OS.
- **Linux/UNIX** → traditionally CLI-based, but has GUIs like **KDE** and **GNOME**.

---

## 3. Touch-Screen Interface

### 📌 Definition

Instead of keyboard or mouse, the **user interacts by touching the screen directly**.

- **Smartphones & Tablets** → primary interface is touch.
- Uses **gestures**: tapping, swiping, pinching, etc.
- Software keyboards appear when typing is required.

### 📌 Examples

- **iPhone / iPad** → Springboard interface.
- **Android** → touch-optimized UI with virtual navigation buttons.
- Touch replaces mouse clicks and keyboard shortcuts.

---

## 4. Choice of Interface

### 📌 Who uses what?

- **Power Users / Sysadmins** → CLI (fast, scriptable, precise).
- **General Users** → GUI (intuitive, less error-prone).
- **Mobile Users** → Touch-screen (portable, natural for handheld devices).

### 📌 Hybrid Systems

- Modern OSs often provide **both CLI and GUI**:
    - **Linux**: CLI + GUI (GNOME/KDE).
    - **Windows**: GUI + PowerShell/Command Prompt.
    - **macOS**: GUI + Terminal (bash/zsh shell).

### 📌 Scripts vs GUIs

- Some OS functions are only accessible via CLI.
- GUIs often hide "advanced" or "rarely used" functions.
- Scripts enable **automation** of tasks that GUIs cannot easily perform.

---

# 🖥️ Practical Demonstrations

### Example 1: CLI (Linux / macOS terminal)

```Bash
# Create a directory and move files
mkdir Projects
mv *.c Projects/

# Compile and run a program
gcc main.c -o main
./main
```

### Example 2: GUI

- Same workflow with a GUI:
    - Open "File Explorer" or "Finder"
    - Create new folder “Projects”
    - Drag all `.c` files into it
    - Double-click `main.c` in an IDE to compile and run

CLI took **3 lines**; GUI took **multiple clicks** but was visually intuitive.

---

# ⭐ Summary

- **CLI**: text-based, powerful, scriptable → favored by experts.
- **GUI**: visual, intuitive, beginner-friendly → favored by general users.
- **Touch Interface**: gesture-based, designed for mobile.
- **Choice of Interface** depends on **use case** and **user preference**.

From the OS’s perspective, **the interface is just a layer**. Underneath, both GUI and CLI commands eventually translate into **system calls to the kernel**.

---

👉 Would you like me to also show you how these different interfaces ultimately connect to **system calls** in the OS (like how `rm file.txt` or a GUI “delete” button both become the same system call to unlink a file)? That way, you’ll see how the interfaces differ **only at the surface level**, but converge inside the kernel.