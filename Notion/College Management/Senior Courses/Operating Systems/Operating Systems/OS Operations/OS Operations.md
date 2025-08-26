For a computer to start running, when it is powered up or rebooted, it need to have an initial program to run.

- Bootstrap program — simple code to initialize the system, load the kernel. Typically, it is stored within the computer hardware in firmware. It initializes all aspects of the system, from CPU registers to device controllers to memory contents. It knows how to load the OS and how to start executing that system. Therefore, it must locate the OS kernel and load it into memory.
- Kernel loads
- Starts system daemons (services provided outside of the kernel) at boot time. They run the entire time the kernel is running. On Linux, the first system program is “systemd” and it starts many other daemons.
- If there are no processes to execute, the OS waits for events. Events are almost always signaled by interrupts.
- Kernel interrupt driven (HW and SW)
    - hardware interrupt by one of the devices
    - Software interrupt (exception or trap)
        - software error
        - req for OS service — system call
        - other process problems include infinite loop, processes modifying each other or the OS

  

# Multiprogramming and Multitasking

  

## Multiprogramming

- Single user cannot always keep CPU and I/O devices busy. Users also typically want to run more than one program at a time.
- Multiprogramming organizes jobs (code and data) so CPU always has one to execute. It increases CPU utilization. In a multiprogrammed system, a program in execution is termed a process.
- A subset of total jobs in system is kept in memory. The OS keeps several processes in memory simultaneously.
- One job selected and run via job scheduling
- When job has to wait, OS switches to another job.

  

## Multitasking

- is a logical extension of batch systems or multiprogramming— the CPU switches jobs so frequently that user scan interact with each job while it is running, creating interactive computing.
    - Response time should be < 1 second
    - Each user has at least one program executing in memory → process
    - If several jobs ready to run at the same time → CPU Scheduling
    - If processes don’t fit in memory, swapping moves them in and out to run
    - virtual memory allows execution of processes not completely in memory

  

# Dual Mode and Multimode Operation

  

- Dual mode operation allows OS to protect itself and other system components since its users share the hardware and software resources of the computer system. It must ensure that an incorrect or malicious program cannot cause other programs or the OS itself to execute incorrectly. The usual approach is to provide hardware support that allows differentiation among various modes of execution.
    - User mode and kernel mode
- Mode bit provided by hardware
    - A bit, called the mode bit is added to the hardware of the computer to indicate the current mode:
        - kernel → 0
        - user → 1
    - It distinguishes between a task executed on behalf of the OS and one that is executed on behalf of the user.
- How do we guarantee that user does not explicitly set the mode bit to kernel?
    - system call changes mode to kernel, return from call resets it to user
- Some machine instructions are designated as privileged are only executable in kernel mode to protect the system from errant users and errant users from one another.
    - The hardware allows privileged instructions to only be executed in kernel mode. if the instruction is not in kernel mode when executing a privileged instruction, the hardware does not execute the instruction and traps it to the OS.
    - The instruction to switch from kernel mode to user and vice versa is a privileged instruction. Some are are also I/O control, timer management, and interrupt management.
- At system boot time, the hardware starts in kernel mode. The OS is then loaded and starts user applications in user mode. Whenever an interrupt occurs, the hardware switches from user mode to kernel mode. Thus, whenever the OS system gains control of the computer, it is in kernel mode. The system always switches to user mode before passing control to a user program.
- System calls provide the means for a user program to ask the OS to perform tasks reserved for the OS on the user program’s behalf. In all forms, it is the method used by a process to request action by the OS. A system call usually takes the form of a trap or interrupt to a specific location in the interrupt vector. This trap can be executed by a generic trap instruction, although some system have a specific syscall instruction to invoke a system call.

![[/image 5.png|image 5.png]]

  

  

## **2. System Calls**

A **system call** is how a user program **asks the OS to do something it can’t do itself**.

### **Why?**

- User programs can't directly access hardware (e.g., read disk, send network packets, draw on screen) because of security and stability.
- Instead, they make a _controlled request_ to the OS via a **system call**.

### **The process**

1. **User code** wants an OS service (e.g., open a file).
2. It **calls a system call function** provided by the OS’s API (like `open()` in Linux or `CreateFile()` in Windows).
3. That function executes a **special CPU instruction**:
    - Could be `trap`, `syscall`, or `int 0x80` (x86 Linux old style).
    - This instruction **switches the CPU to kernel mode**.
4. CPU jumps to the OS’s **system call handler** (found via the **interrupt vector table**).
5. The OS:
    - Checks parameters (safety check).
    - Performs the requested operation.
    - Returns the result to the user process.
6. CPU switches back to **user mode**.

---

## **Example – System Call in Action**

### **Linux x86-64 Assembly Example**

```Assembly
; Write "Hello" to stdout using a Linux system call

section .data
    msg db "Hello", 0xA
    len equ $ - msg

section .text
    global _start

_start:
    mov rax, 1          ; syscall number for sys_write
    mov rdi, 1          ; file descriptor 1 = stdout
    mov rsi, msg        ; pointer to message
    mov rdx, len        ; message length
    syscall             ; invoke kernel

    ; Exit the program
    mov rax, 60         ; syscall number for sys_exit
    xor rdi, rdi        ; exit code 0
    syscall
```

**What happens here:**

- `syscall` instruction → CPU switches from **user mode** to **kernel mode**.
- The OS checks `rax` to know which service is requested (1 = write).
- OS executes `write()` in kernel space, writes "Hello" to terminal.
- Returns to **user mode**.

---

## **Multi-Mode Extension**

Some CPUs have **more than two modes**:

- **Rings** in x86: Ring 0 (kernel) → Ring 3 (user).
- **Virtualization mode**: For hypervisors that run virtual machines.
- **ARM**: Has several modes like Supervisor, IRQ, FIQ, etc.

---

  

  

  

Not every system call directly maps 1-to-1 to a system interrupt vector — but many _do_ go through an interrupt or trap mechanism under the hood.

Let me break it down step-by-step so it’s clear how it works.

---

## **1. The OS API vs. the Interrupt Vector Table**

- **OS API**
    
    When you write code like:
    
    ```C
    write(fd, buffer, size);
    ```
    
    you’re using a _system call_, which is part of the OS’s API to user programs.
    
- **Interrupt Vector Table (IVT)**
    
    This is a table of addresses (function pointers) for _hardware_ and _software_ interrupts.
    
    - Hardware interrupts come from devices (keyboard, timer, network card, etc.).
    - Software interrupts/traps are instructions from the CPU that switch to kernel mode so the OS can do privileged operations.

---

## **2. How a System Call Uses an Interrupt**

On many architectures (x86, ARM, RISC-V), a system call is implemented with:

- A **special instruction** (`int 0x80`, `syscall`, or `svc`)
- This triggers a **software interrupt/trap**
- The CPU looks up the corresponding handler in the interrupt vector table
- Control jumps into kernel mode, where the OS runs the relevant code

Example on Linux x86 (old style):

```Assembly
mov eax, 4        ; syscall number for write()
mov ebx, 1        ; fd = 1 (stdout)
mov ecx, msg      ; pointer to message
mov edx, len      ; message length
int 0x80          ; trigger software interrupt -> jumps to syscall handler
```

---

## **3. Is there a unique vector for every syscall?**

No.

- Usually **all syscalls share the same trap vector** (one entry in the IVT).
- The syscall number in a register (like `EAX` above) tells the kernel which specific syscall you want.
- Inside the kernel:
    
    ```C
    syscall_table[] = {
        sys_read, sys_write, sys_open, /* ... */
    };
    handler() {
        int num = get_syscall_number();
        syscall_table[num](args);
    }
    ```
    

So:

- **Vector table**: One entry for "system calls"
- **Syscall table**: Many entries, one per OS API function

---

## **4. Hardware interrupts vs. system calls**

- Hardware interrupt: Triggered by devices, mapped to different vectors in IVT
    
    Example:
    
    - Timer interrupt: Vector 32
    - Keyboard interrupt: Vector 33
- System call (software interrupt): Usually one vector, e.g., vector 128 (`int 0x80`)

---

  

  

  

Alright — timers are one of the **core mechanisms** the OS uses to enforce CPU control, implement multitasking, and prevent a single process from taking over the machine indefinitely.

Let’s unpack **1.4.3 Timer** in detail, and I’ll also give you code examples from both **bare-metal** and **Linux kernel** contexts so you see exactly how it works in practice.

---

## **1. Purpose of the Timer in OS Control**

- **Problem**: If a user program runs in an infinite loop (accidentally or maliciously), without calling back into the OS, the OS would never regain control.
- **Solution**: The hardware timer generates periodic **interrupts** that:
    1. Switch the CPU from **user mode → kernel mode**
    2. Give the OS a chance to:
        - Update scheduling data
        - Preempt the running process
        - Switch to another process if needed

This is the foundation of **preemptive multitasking**.

---

## **2. Hardware Implementation**

Two common implementations:

1. **Fixed Interval** (e.g., 60 times per second → 16.67ms per tick)
    
    OS always gets control at the same intervals.
    
2. **Variable Interval** (programmable countdown timer)
    - The OS writes a value into a timer register
    - A clock decrements it
    - When it hits zero, a timer interrupt occurs

> On x86, the Programmable Interval Timer (PIT) or HPET is often used.
> 
> On ARM, the **Generic Timer** or **SysTick** is common.

---

## **3. OS Behavior**

When starting a user process:

1. **Set the timer** (privileged instruction — only in kernel mode)
2. **Switch to user mode**
3. **When the timer fires** → CPU jumps to **timer interrupt handler** in kernel mode
4. OS can:
    - Kill the process (if needed)
    - Reset the timer
    - Run scheduler to pick next process

---

## **4. Example — Bare-Metal C (x86 PIT)**

```C
// I/O ports for the PIT
\#define PIT_COMMAND 0x43
\#define PIT_CHANNEL0 0x40
\#define PIT_FREQUENCY 1193182 // Hz

void pit_init(uint32_t frequency) {
    uint16_t divisor = PIT_FREQUENCY / frequency;

    // Command byte: channel 0, low/high byte, mode 3 (square wave)
    outb(PIT_COMMAND, 0x36);
    outb(PIT_CHANNEL0, divisor & 0xFF);       // low byte
    outb(PIT_CHANNEL0, (divisor >> 8) & 0xFF); // high byte
}

void pit_handler() {
    // This function is called on every timer interrupt
    scheduler_tick();
}

// During OS boot:
pit_init(1000); // 1000 Hz → 1ms per tick
register_interrupt_handler(IRQ0, pit_handler);
```

Here:

- `outb` writes to I/O ports (privileged instruction)
- IRQ0 is the hardware interrupt vector for the PIT
- The handler runs **every millisecond** and drives the scheduler

---

## **5. Example — Linux Kernel**

Linux uses **HZ** to determine ticks per second:

- `HZ = 250` → 1 tick every 4ms
- Global `jiffies` increments every tick

You can read jiffies in a kernel module:

```C
\#include <linux/module.h>
\#include <linux/init.h>
\#include <linux/jiffies.h>

static int __init timer_demo_init(void) {
    printk(KERN_INFO "System has been up for %lu ticks\n", jiffies);
    return 0;
}

static void __exit timer_demo_exit(void) {
    printk(KERN_INFO "Module exiting at jiffies=%lu\n", jiffies);
}

module_init(timer_demo_init);
module_exit(timer_demo_exit);
MODULE_LICENSE("GPL");
```

---

## **6. Privileged Nature**

- Modifying the hardware timer is a **privileged instruction**.
- Only the OS kernel can set/reset the timer registers.
- If a user-mode program tried, the CPU would raise a **protection fault**.

---

## **7. How This Relates to Dual-Mode Operation**

Timers are one of the **few guaranteed ways** the OS can regain control from user mode **without the program’s cooperation**.

- System calls are cooperative — the user process _chooses_ to call them.
- Timer interrupts are preemptive — the OS can stop any process, any time.

---

## **8. Summary**

- Timers generate **interrupts** at regular intervals
- They are essential for **preemptive multitasking**
- Their configuration is privileged
- In Linux, `HZ` and `jiffies` are the key timing concepts
- Without a timer, a single program could monopolize the CPU forever

---

  

# Process Management

- A process is a program in execution. It is a unit of work within the system. Program is a passive entity; a process is an active entity. A process is an instance of a program in execution.
- Processes needs resources to accomplish its task.
    - these are typically allocated while it is running
    - various initialization data may also be passed
- Process termination requires reclaim of any reusable resources
- Single-threaded process has one program counter specifying location of next instruction to execute
    - Process executes instructions sequentially, one at a time, until completion
- Multi-threaded process has one PC per thread
- typically a system has many processes, some are user processes, some are OS processes running concurrently on one or more CPUs
    - concurrency can be achieved by multiplexing on a single CPU core—or in parallel across multiple CPU cores.

The operating system is responsible for the following activities in connection with process management:  
• Creating and deleting both user and system processes  
• Scheduling processes and threads on the CPUs  
• Suspending and resuming processes  
• Providing mechanisms for process synchronization  
• Providing mechanisms for process communication

- Providing mechanisms for deadlock handling

  

---

### 🔹 Program vs. Process

- **Program** = passive (just code sitting in a file on disk).
- **Process** = active (that program _running_ in memory, using CPU, RAM, I/O, etc.).

👉 Example:

- `chrome.exe` saved on disk is a **program**.
- Double-clicking Chrome launches **a process** (actually many).

So processes are the _unit of work_ the OS manages.

---

### 🔹 Resources Needed

A process needs:

- **CPU time** (to execute instructions)
- **Memory** (to store instructions & data)
- **Files** (like configs, inputs, outputs)
- **I/O devices** (keyboard, display, network, etc.)

The OS allocates these when the process is created, and reclaims them when the process ends.

---

### 🔹 Process Execution

- **Single-threaded process**: one program counter (PC), executes sequentially, one instruction at a time.
- **Multi-threaded process**: multiple PCs, one for each thread (so different threads can execute different parts of the program concurrently).

---

### 🔹 Multiplexing vs Parallelism

- On **a single CPU core**, multiple processes share it by _time-sharing_.
    - This is where **scheduling** comes in.
    - Example: Round Robin scheduling gives each process a time slice (quantum) of CPU before switching.
- On **multiple CPU cores**, processes can _actually run in parallel_.

👉 Scheduling is the OS’s way of deciding **who gets the CPU next** when there are multiple runnable processes.

---

### 🔹 OS Responsibilities in Process Management

1. **Creating & deleting processes** → when you open or close programs.
2. **Scheduling** → deciding which process/thread runs next (this is where round robin, FCFS, priority scheduling, etc. come in).
3. **Suspending & resuming processes** → like when you `pause`/`sleep` a process.
4. **Process synchronization** → making sure multiple processes/threads don’t mess each other up (e.g., race conditions).
5. **Process communication** → IPC (Inter-Process Communication), like sockets, shared memory, pipes, etc.

---

  

# Memory Management

Main memory is a large array of bytes. Each byte has its own address. Main memory is a repository of quickly accessible data shared by the CPU and I/O devices.

---

# 📘 Lecture: Memory Management (Section 1.5.2)

## 1. Why Memory Management Exists

Main memory (RAM) is a **finite, volatile, fast storage medium** directly accessible by the CPU. Programs must live in memory to execute because:

- CPU fetches **instructions** from memory.
- CPU reads/writes **data** from memory.
- I/O devices cannot be accessed directly; data must first be staged in RAM.

The challenge:

- RAM is **limited**, but users want to run many programs simultaneously.
- Programs don’t politely fit into neat, fixed memory slots.
- Without organization, programs could overwrite each other’s memory.

👉 This is why the **Operating System (OS)** plays the role of **Memory Manager**.

---

## 2. OS Responsibilities in Memory Management

As the text says, the OS is responsible for three main activities:

1. **Tracking** which parts of memory are used and by whom.
    
    → Data structure: often a **bitmap** or a **linked list of free/used blocks**.
    
    ```Plain
    Example bitmap:
    [1][1][0][0][1][0][0][0]
    1 = allocated, 0 = free
    ```
    
2. **Allocating / Deallocating memory space**.
    
    → When a process requests memory, the OS finds a free block and assigns it.
    
    → When the process exits, the OS reclaims that memory.
    
3. **Deciding what to move in/out of memory**.
    
    → In modern systems, OS swaps pages between RAM and disk (“paging”) when RAM is full.
    

---

## 3. Memory Addressing: Absolute vs. Virtual

Originally, programs were mapped to **absolute addresses**. Example:

```Assembly
; CPU thinks this is absolute memory
MOV AX, [1000h]   ; load from address 0x1000
```

But that causes issues:

- What if two programs both want to use `0x1000`?
- What if program size changes?

💡 Solution: **Virtual Memory**.

- Each process thinks it has its own address space (`0x0000 - 0xFFFF` for example).
- The OS + **MMU (Memory Management Unit)** translates _virtual addresses_ to _physical addresses_.

Example:

```Plain
Process A (Virtual) 0x1000 → (Physical) 0x3A1000
Process B (Virtual) 0x1000 → (Physical) 0x5F2000
```

Thus, programs are isolated and memory is shared fairly.

---

## 4. Memory Management Schemes

Different **algorithms/schemes** exist for memory management, each with trade-offs.

### (a) **Single Contiguous Allocation**

- Only one program in memory at a time.
- Simple but wastes CPU (no multitasking).

### (b) **Fixed Partitioning**

- Memory is divided into fixed slots.
- Multiple programs run, but fragmentation occurs (unused memory inside slots).

```Plain
RAM:
[OS][ P1 ][ P2 ][free][ P3 ]
```

### (c) **Dynamic Partitioning**

- Allocate memory dynamically based on program size.
- Problem: **external fragmentation** (holes between allocations).

### (d) **Paging** (most common today)

- Memory divided into fixed-size **pages** (4 KB typical).
- Process memory is stored in disjoint pages.
- OS uses **page tables** to map virtual pages → physical frames.

Example mapping table:

```Plain
Virtual Page | Physical Frame
0x0000       | 0x2000
0x0001       | 0x4500
0x0002       | swapped to disk
```

### (e) **Segmentation**

- Divide memory logically (code, data, stack).
- More natural for programmers, but complex to manage.

### (f) **Virtual Memory with Demand Paging**

- The system pretends it has _infinite memory_.
- OS loads only the needed pages into RAM.
- If a page is missing → **Page Fault** → bring it from disk.

---

## 5. CPU + OS + Hardware Interaction

- **CPU generates virtual addresses** when executing instructions.
- **MMU translates** them to physical addresses (via page tables).
- If not in RAM → **OS intervenes** (page fault handler).
- OS may evict another page (replacement policies like LRU).

---

## 6. Code-Level Example (Linux)

Here’s a **user-level simulation** of memory allocation in C:

```C
\#include <stdio.h>
\#include <stdlib.h>
\#include <string.h>

int main() {
    char *buffer1 = malloc(20);   // request 20 bytes
    char *buffer2 = malloc(30);   // request 30 bytes

    strcpy(buffer1, "Hello Memory!");
    printf("%s\n", buffer1);

    free(buffer1);   // release memory
    free(buffer2);

    return 0;
}
```

- `malloc()` → user request.
- The C library forwards it to the OS via `brk()` or `mmap()` system calls.
- OS allocates physical memory frames (or pages them in).
- `free()` returns memory to the OS/allocator.

---

## 7. Scheduling Connection

Now, to your earlier question about **Round Robin scheduling**:

That’s **CPU scheduling** (who gets to use the CPU next).

Memory management interacts with it because:

- A process can’t even _enter_ the CPU queue if it doesn’t have memory loaded.
- If memory is full, the OS may **swap out a process** to disk, delaying its scheduling.

So scheduling + memory management are tightly connected.

---

## 8. Why It’s Hard

- Memory is finite and shared → requires fair but efficient allocation.
- Too much fragmentation = waste.
- Too much swapping = slowdown (disk is millions of times slower than RAM).
- Security must be enforced: one process must never read another’s memory.

That’s why **memory protection**, **isolation**, and **virtual memory** are core OS features.

---

✅ **Summary**:

- Memory is a scarce resource, shared between CPU and I/O.
- OS tracks usage, allocates/deallocates, and moves data in/out.
- Modern systems use **paging + virtual memory** to make memory appear infinite and isolated.
- CPU scheduling and memory management are intertwined — no memory, no execution.

---

Would you like me to next **illustrate the hardware side** with an example of how the **MMU + page tables** actually work step by step when the CPU executes an instruction (with a small C example showing a page fault)?

  

  

# File System Management

> [!important] A file is a collection of related information defined by its creator.

Perfect — you’ve just landed on one of the **core pillars of an OS**: the file system. Let me give you a **deep, professor-level lecture** on **file-system management**.

---

## 🔹 1. The Problem File Systems Solve

At the hardware level, storage devices (like HDDs, SSDs, USBs, optical disks) are just **arrays of bytes or blocks**. For example, an SSD is divided into _pages_ and _blocks_, and a disk is divided into _sectors_ and _tracks_.

But raw bytes/blocks are not convenient for humans:

- Users want to work with _files_ (like `essay.docx` or `song.mp3`), not block numbers.
- Applications need **logical organization** (directories, permissions, extensions).
- Different devices (SSDs, HDDs, tapes, CDs) all have different access methods.

👉 The **Operating System (OS)** provides an **abstraction layer** called the **file system** that:

- Hides the messy physical details of storage.
- Presents a **uniform, logical view**: “a file is just a sequence of bytes.”
    - Abstracts physical properties to logical storage unit: **file**
- Provides mechanisms for naming, accessing, protecting, and storing files.

---

## 🔹 2. File as an Abstraction

- **Definition**: A file is a **named collection of related information**.
- Files can contain:
    - Programs (source code, executables).
    - Data (text, numbers, images, video, audio).
- Files may be:
    - **Unstructured**: e.g., `.txt` (just a stream of characters).
    - **Structured**: e.g., `.mp3` or `.jpg` (with headers, metadata, strict format).

👉 The OS ensures we don’t care _how_ a file is physically stored; we just open, read, write, and close.

---

## 🔹 3. File System Functions

The OS’s **file-system management layer** is responsible for:

1. **Creating and deleting files**
    - `open()`, `create()`, `remove()` in UNIX-like systems.
    - Keeps metadata in structures like **inodes** (UNIX/Linux) or **FAT tables** (Windows FAT32).
2. **Creating and deleting directories**
    - Directories are special files that contain mappings from **names → file references (inodes, clusters, etc.)**.
3. **Primitives for manipulation**
    - `read()`, `write()`, `append()`, `seek()`, etc.
    - Supports random and sequential access.
4. **Mapping files onto mass storage**
    - The **File Allocation Method**:
        - **Contiguous allocation** (good performance, but fragmentation issues).
        - **Linked allocation** (flexible, but slower).
        - **Indexed allocation** (used in UNIX inodes, NTFS MFT).
5. **Backup and recovery**
    - Protects against data loss via snapshots, journaling, RAID, or external backup.

---

## 🔹 4. Hierarchical Organization

- Directories organize files in a **tree structure**:
    
    ```Plain
    / (root)
     ├── home/
     │   ├── alice/
     │   │   └── essay.docx
     │   └── bob/
     └── var/
         └── log/
    ```
    
- This makes it scalable for millions of files.

---

## 🔹 5. Security and Access Control

When multiple users exist, we must enforce **permissions**:

- **Access Control Lists (ACLs)** – e.g., user `alice` can `read`, `bob` can `write`.
- **Unix-style permissions** (`rwxr-xr--`).
- Modern file systems (NTFS, ext4, ZFS) enforce permissions at the OS level.

---

## 🔹 6. Storage Devices and Their Characteristics

The OS must handle very different devices:

- **Hard Disk Drives (HDDs)**: mechanical, random access, sectors.
- **SSDs**: block/page-based, wear leveling.
- **Tapes/Optical Media**: sequential access.
- **Tertiary storage**: slower, offline archives.

The **file system driver** abstracts these differences so the same `open("file.txt")` works regardless of the medium.

---

## 🔹 7. Example: UNIX/Linux

Linux uses:

- **VFS (Virtual File System)** – provides a uniform interface (`open`, `read`, etc.).
- **Concrete File Systems** – ext4, XFS, Btrfs, etc. implement the actual mapping.
- **Inode structure**:
    
    ```C
    struct inode {
        mode_t mode;         // File type and permissions
        uid_t uid;           // Owner ID
        gid_t gid;           // Group ID
        off_t size;          // File size
        time_t atime;        // Last access time
        time_t mtime;        // Last modification time
        time_t ctime;        // Last inode change time
        block_pointers[];    // Pointers to data blocks
    };
    ```
    

---

## 🔹 8. Example Code – File Manipulation in C (POSIX)

Here’s how the OS exposes file management primitives via **system calls**:

```C
\#include <stdio.h>
\#include <fcntl.h>
\#include <unistd.h>

int main() {
    int fd;
    char buffer[100];

    // Create or open a file
    fd = open("example.txt", O_CREAT | O_RDWR, 0644);

    // Write data
    write(fd, "Hello, File System!", 19);

    // Move file pointer to beginning
    lseek(fd, 0, SEEK_SET);

    // Read data
    read(fd, buffer, 19);
    buffer[19] = '\0';
    printf("Read: %s\n", buffer);

    // Close file
    close(fd);

    return 0;
}
```

👉 Here:

- `open()` → creates/opens file (OS allocates an entry in the file table).
- `write()` → OS maps logical file offset → physical disk block.
- `lseek()` → changes file pointer (supports random access).
- `read()` → loads from disk to memory.
- `close()` → releases the file descriptor.

This is **system call → OS → file system driver → disk controller** in action.

---

## 🔹 9. Why It Matters

- File-system design affects **performance** (SSD vs HDD optimization).
- Affects **reliability** (journaling prevents corruption after crashes).
- Affects **security** (permission models).
- Affects **user experience** (hierarchical structure, metadata, search).

Without a file system, your computer would just be a sea of meaningless blocks of data.

---

✅ **Summary**:

- The OS abstracts raw storage into logical files.
- It manages creation, deletion, organization, access control, and backup.
- Behind the scenes, files are mapped onto blocks/sectors via allocation methods.
- Users/applications see a clean logical view, while the OS handles the complexity.

---

Would you like me to also walk you through **how different file allocation methods (contiguous, linked, indexed, journaling)** actually work internally (with diagrams and maybe a C-like simulation)? That’s where the _real magic_ of file systems is.

  

  

  

---

# 📖 Lecture: Mass-Storage Management (Section 1.5.4)

### 1. **Why Mass-Storage Management Matters**

- **Main memory** (RAM) is fast but volatile (contents lost when power is off).
- Programs and data must persist across reboots → stored on **non-volatile storage** (NVM devices like SSDs, HDDs).
- Performance of the entire system often depends on how efficiently storage is managed. It can become the bottleneck of the system.
    
    👉 Example: A slow disk scheduler or poor allocation strategy can make everything—from booting, to opening files, to database queries—painfully slow.
    

Thus, **the OS plays the role of storage manager**, ensuring:

- Efficient use of storage
- Fair and safe sharing among processes
- High throughput and low latency

---

### 2. **Core Activities of Storage Management**

### (a) **Mounting and Unmounting**

- **Mounting** = making a storage device accessible to the file system.
    
    Example: inserting a USB and the OS maps it to `/mnt/usb` (Linux) or assigns `E:\` (Windows).
    
- **Unmounting** = safely removing, ensuring all writes are flushed.
- The OS provides system calls (`mount`, `umount`) and manages consistency.

👉 Why? If the OS didn’t control mounting, processes could access raw hardware directly, risking corruption.

---

### (b) **Free-Space Management**

- Storage is divided into **blocks** (small fixed-size units).
- When files are deleted, their blocks become free.
- OS keeps track of **which blocks are free vs. used**, using:
    - **Bitmaps** (1 = free, 0 = allocated)
    - **Linked lists** of free blocks
    - **Grouping or counting** methods

👉 Efficient free-space management prevents **fragmentation** and ensures fast allocation.

---

### (c) **Storage Allocation**

- How do we assign blocks to files?
- Strategies:
    - **Contiguous allocation** (file occupies consecutive blocks; fast but causes external fragmentation)
    - **Linked allocation** (blocks linked like a chain; avoids fragmentation but slower for random access)
    - **Indexed allocation** (use an index block listing all file blocks; common in Unix `inode`)

👉 The OS must balance **speed vs. flexibility**.

---

### (d) **Disk Scheduling**

- Disks (especially HDDs) are mechanical → seek time matters.
- Scheduling algorithms decide **which I/O request to serve next**:
    - **FCFS** (First-Come, First-Served) – fair but slow
    - **SSTF** (Shortest Seek Time First) – reduces average seek
    - **SCAN / Elevator** – sweeps back and forth like an elevator
    - **C-SCAN** (Circular SCAN) – always goes in one direction, improves fairness

👉 With SSDs, scheduling is less about seek time (since no moving parts) and more about **wear leveling and queue management**.

---

### (e) **Partitioning**

- Storage devices can be divided into **partitions** (logical subdivisions).
- Each partition can hold:
    - An OS
    - A file system
    - Swap space
- Modern systems also use **logical volumes** (via LVM, ZFS) for flexible resizing.

👉 This allows multi-boot systems, isolation of data, and better management.

---

### (f) **Protection**

- Just like memory, storage needs **access control**.
- OS enforces permissions (read, write, execute) at the **file system level**.
    
    Example:
    
    - Unix permissions (`chmod 644 file.txt`)
    - Windows ACLs
- Ensures that one process cannot corrupt another’s data.

---

### 3. **Tertiary Storage**

- Slower, cheaper, higher capacity (e.g., tapes, Blu-ray, archival systems).
- Used for:
    - **Backups**
    - **Rarely accessed data**
    - **Archival for decades** (medical, legal, scientific data)
- Management varies:
    - Some OSes handle mounting/unmounting automatically
    - Others leave it to applications (e.g., enterprise backup software)

👉 While not performance-critical, tertiary storage management ensures **reliability** and **data durability**.

---

### 4. **Efficiency and System Performance**

- Efficient storage management can drastically affect system throughput.
- Example:
    - Database on an HDD → disk scheduling + caching + allocation = performance difference of **10x or more**.
    - On SSD → allocation and wear leveling become crucial to **device lifespan**.

---

### 5. **Programming Perspective**

OS developers implement storage management in the **I/O subsystem**:

- **Device drivers** handle low-level commands to disks.
- **File system layer** translates user-level operations (open, read, write) into block-level I/O.
- **Disk scheduler** decides ordering of requests.
- **Buffer cache** reduces I/O overhead by keeping frequently used blocks in RAM.

👉 Example (Linux syscall interface):

```C
int fd = open("/mnt/data/file.txt", O_RDONLY);
read(fd, buffer, 1024);
close(fd);
```

- These high-level calls are translated by the OS into block reads via the storage driver.
- Under the hood: `read()` → File system lookup → Disk scheduler → Driver → Controller → Disk.

---

### 6. **Analogy**

Think of the OS storage subsystem as **an airport baggage system**:

- **Passengers (files)** are assigned **bins (blocks)**.
- The **airport (OS)** decides where bins go (allocation).
- **Conveyor belts (disk scheduler)** move bins efficiently.
- **Security (protection)** ensures only the right passenger picks up their bag.
- **Storage rooms (tertiary storage)** hold rarely used bags long-term.

---

✅ **In summary:**

Mass-storage management is the OS’s backbone for persistence. It ensures that storage is efficiently allocated, fairly scheduled, safely protected, and consistently maintained. Without it, a computer would be a fast calculator that forgets everything once powered off.

---

Would you like me to **extend this into actual C code** showing how an OS kernel might implement something like **free-space management (bitmap allocation)**, so you see how it looks in practice?

  

  

  

---

# 📖 Lecture: Cache Management (Section 1.5.5)

---

## 1. **What is Caching?**

Caching is the practice of keeping frequently used data in **smaller but faster storage** to speed up access.

- Instead of always going to a **slow storage device** (like disk), we first check a **faster temporary copy** (cache).
- If found → **cache hit** (fast).
- If not found → **cache miss**, fetch from slower storage, and place a copy in cache.

👉 The principle relies on **locality of reference**:

- **Temporal locality** → recently used data is likely to be used again soon.
- **Spatial locality** → data near recently used data is also likely to be used soon.

---

## 2. **Memory Hierarchy**

Caches exist at **multiple levels** in a storage hierarchy:

|Level|Typical Size|Access Time (ns)|Managed By|
|---|---|---|---|
|**Registers**|< 1 KB|~0.25–0.5|Compiler|
|**Cache (L1/L2/L3)**|< 16 MB|0.5–25|Hardware|
|**Main Memory (RAM)**|< 64 GB|80–250|OS|
|**SSD (NVM)**|< 1 TB|25,000–50,000|OS|
|**HDD**|< 10 TB|5,000,000+|OS|

👉 The **higher** you go = smaller + faster.

👉 The **lower** you go = larger + slower.

The OS plays a role in **main memory ↔ disk caching**, but **CPU cache ↔ RAM** is handled by **hardware**.

---

## 3. **Cache Operations**

Example: Incrementing an integer `A` stored in a file on disk.

- Step 1: Disk block containing `A` → loaded into main memory.
- Step 2: Value of `A` → copied to CPU cache.
- Step 3: Value of `A` → loaded into register.
- Step 4: Register increments `A`.
- Step 5: Updated `A` → must be written back to cache → memory → disk.

👉 At any given time, **multiple copies** of `A` may exist in the hierarchy. This leads to the **cache coherency problem**.

---

## 4. **Cache Coherency**

- In a **single-process, single-core** system, coherency isn’t much of an issue → always use the “closest” copy.
- In **multitasking**, multiple processes may want `A`. The OS must ensure they see the **latest value**.
- In **multiprocessors (multi-core CPUs)**:
    - Each CPU core has its own cache.
    - If one core modifies `A`, all other caches holding `A` must be updated or invalidated.
    - This is handled by **cache coherence protocols** (MESI, MOESI), usually implemented in hardware.

---

## 5. **Replacement Policies**

Caches are **limited in size** → not everything fits. The OS (or hardware) must choose what to evict when new data comes in.

Common replacement algorithms:

- **LRU (Least Recently Used)** → evict the data that hasn’t been used for the longest time.
- **FIFO (First-In, First-Out)** → evict the oldest cached block.
- **LFU (Least Frequently Used)** → evict data with the fewest accesses.
- **Random** → simple, sometimes surprisingly effective.

👉 The choice of policy can have a huge impact on performance (OS textbooks dive into this in memory management chapters).

---

## 6. **Explicit vs. Implicit Caching**

- **Implicit caching** → handled automatically by hardware (CPU instruction/data caches). The OS has no control.
- **Explicit caching** → OS or application explicitly manages caching.
    - Example: **Page cache** in Linux: when you read a file, it’s cached in RAM for future reads.
    - Example: Database software often implements its own caching layer.

---

## 7. **Distributed Caching**

- In distributed systems (e.g., cloud services, web apps):
    - Multiple copies of the same file or object may exist across machines.
    - Updating one copy requires **propagating changes** to others to maintain consistency.
    - Approaches:
        - **Write-through caching** → update cache + main storage simultaneously.
        - **Write-back caching** → update cache first, main storage later (faster, but risk of inconsistency).
        - **Replication protocols** (eventual consistency, strong consistency).

👉 This is where cache management meets **network protocols** and **distributed databases**.

---

## 8. **Real-World Examples**

- **CPU caches**: Handled by hardware (L1, L2, L3).
- **Page cache in OS**:
    - Linux keeps file data in memory so that the next `read()` is fast.
    - Command `free -h` shows how much memory is used for cache.
- **Web browsers**: Cache images, scripts, and pages for faster reload.
- **Databases (e.g., MySQL, Redis)**: Cache query results to avoid expensive recomputation.

---

## 9. **Programming Perspective**

Let’s look at a simplified **software-managed cache** in C (like an OS page cache):

```C
\#include <stdio.h>
\#include <stdlib.h>

\#define CACHE_SIZE 4

typedef struct {
    int block_id;
    int data;
} CacheEntry;

CacheEntry cache[CACHE_SIZE];
int cache_count = 0;

// Simple LRU replacement
int find_in_cache(int block_id) {
    for (int i = 0; i < cache_count; i++) {
        if (cache[i].block_id == block_id) {
            printf("Cache HIT: Block %d\n", block_id);
            return cache[i].data;
        }
    }
    return -1; // Not found
}

void add_to_cache(int block_id, int data) {
    if (cache_count < CACHE_SIZE) {
        cache[cache_count++] = (CacheEntry){block_id, data};
    } else {
        // Replace first entry (FIFO policy for simplicity)
        for (int i = 1; i < CACHE_SIZE; i++) {
            cache[i-1] = cache[i];
        }
        cache[CACHE_SIZE-1] = (CacheEntry){block_id, data};
    }
    printf("Cache MISS: Loaded block %d\n", block_id);
}

int main() {
    // Simulating reading blocks
    int blocks[] = {1, 2, 3, 1, 4, 2, 5};
    int n = sizeof(blocks)/sizeof(blocks[0]);

    for (int i = 0; i < n; i++) {
        int data = find_in_cache(blocks[i]);
        if (data == -1) {
            // Simulate fetching data from disk
            add_to_cache(blocks[i], blocks[i] * 10);
        }
    }

    return 0;
}
```

👉 This toy program simulates **cache hits** and **misses**, like an OS page cache. In real OS code, the replacement strategy could be **LRU** or something more sophisticated.

---

## 10. **Summary**

- Caching leverages **locality** to boost performance.
- OS manages **main memory ↔ disk cache**, while hardware manages **CPU cache ↔ RAM**.
- **Replacement policies** (LRU, FIFO, LFU) determine what stays in cache.
- **Consistency** (cache coherence, distributed cache sync) is a major challenge.
- Proper cache management can make systems run **orders of magnitude faster**.

---

✅ In short: caching is the **glue** that allows slow storage systems to feel “fast” to the user. It’s a tradeoff between **speed, space, and consistency**.

---

Would you like me to also walk you through how **Linux manages its page cache** (with actual system calls like `mmap`, `read`, and `sync`) so you see cache management in a real OS?

  

  

  

---

## 1. The Role of I/O System Management

The OS provides a **uniform interface** to input/output (I/O) devices, abstracting away their hardware-specific details.

- From a programmer’s perspective: reading a file is as simple as calling `read()`.
- Under the hood: that might involve spinning a hard disk, waiting for the right block to pass under the read head, transferring data via DMA (Direct Memory Access), and then signaling completion via an interrupt.

The OS handles all of that, making I/O **transparent, efficient, and reliable**.

---

## 2. Major Components of the I/O Subsystem

The I/O subsystem is typically broken down into **layers**:

### (a) **Memory-Management Component**

This deals with how data moves between devices and memory. Key techniques include:

- **Buffering**
    - A buffer is temporary storage used when moving data between two devices or between a device and an application.
    - Why? Because devices and CPUs operate at different speeds. For example, a printer prints a line every few milliseconds, while the CPU can generate data thousands of times faster. A buffer smooths out this mismatch.
- **Caching**
    - Frequently used data is stored in a faster memory (like RAM) to reduce the number of slow device accesses.
    - Example: Disk blocks are cached in RAM so repeated file accesses don’t always hit the slow disk.
- **Spooling (Simultaneous Peripheral Operations On-Line)**
    - Instead of sending data directly to a slow device (like a printer), the OS writes the output to a spool (usually a disk file).
    - The printer driver then reads from this spool at its own pace.
    - This allows multiple processes to "print" at the same time — the OS queues jobs and sends them one by one to the physical printer.

---

### (b) **General Device-Driver Interface**

The OS provides a **standardized API** for device access. Applications don’t interact with hardware directly; they use OS calls.

- Example: In UNIX/Linux, everything is treated as a "file". Whether it’s a keyboard, a hard disk, or even a network socket, you use `open()`, `read()`, `write()`, and `close()`.
- This uniform interface is crucial: an app doesn’t care if it’s reading from an SSD or a USB stick — the OS hides the difference.

---

### (c) **Device Drivers**

- These are specialized modules of the OS that know how to interact with specific hardware.
- Example: A **keyboard driver** interprets raw scan codes from the keyboard into characters; a **disk driver** knows how to position the read/write head and request blocks.
- Only the driver “knows the peculiarities” of the hardware (timing, control registers, protocols).

The layered design means:

1. Applications → OS calls (uniform API)
2. OS I/O subsystem → general driver interface
3. Driver → device hardware

---

## 3. How I/O Works in Practice

Let’s say you open a text file:

1. **Application call**: `read(file_descriptor, buffer, size)`
2. **OS I/O subsystem**: Maps that request to a file system and a block device.
3. **Driver interaction**: The disk driver sends commands to the disk controller.
4. **Hardware execution**: The disk retrieves the requested blocks and uses DMA to transfer them into RAM.
5. **Interrupt handler**: When done, the device raises an interrupt, and the OS resumes the application with the data filled in the buffer.

All the complexity of step 3–5 is invisible to the programmer.

---

## 4. Efficiency Concerns

I/O is often the **slowest part of a computer system** (compared to CPU and memory). The OS must therefore carefully optimize:

- **Overlap computation with I/O** → using interrupts and DMA, so the CPU can work while I/O happens in the background.
- **Caching and buffering** → reduce expensive device accesses.
- **Scheduling** → just like CPU scheduling, I/O requests (especially disk I/O) are ordered efficiently to reduce seek times and waiting.

---

## 5. Connection to Interrupts and Device Management

You might notice this ties back to our earlier discussion:

- I/O heavily depends on **interrupt handlers** — without them, the CPU would have to waste cycles polling devices.
- Each device is managed as a resource: the OS allocates it, synchronizes access, and ensures protection (e.g., two processes can’t directly write to the same device without coordination).

---

## 6. Looking Ahead

Later OS chapters (like Chapter 12 mentioned in your text) dive into:

- **How I/O scheduling is done** (like disk head scheduling algorithms: FCFS, SSTF, SCAN, C-SCAN).
- **How devices and buses are structured** (PCIe, USB, NVMe).
- **How protection and permissions work** (e.g., user vs. kernel mode for device access).

---

✅ **In summary:**

The I/O system management is the **bridge** between abstract program requests (read, write, print) and the messy, diverse, asynchronous world of hardware devices. The OS hides the details, improves efficiency with buffering/caching/spooling, and relies on device drivers plus interrupts to make the whole system work smoothly.

---

Would you like me to **illustrate this with a flow diagram** (application → OS → driver → device → interrupt → OS → application) so you can see the data path more clearly?

  

  

# Security

Perfect — now let’s take a deep dive into **Section 1.6: Security and Protection**. This is one of the most important areas in operating systems, because even if everything else (CPU scheduling, I/O, memory management) works flawlessly, a system is useless if it cannot **protect resources** and **secure data**.

---

# 🔐 1. Protection vs Security: The Big Picture

These two terms are related but not identical:

- **Protection** = _internal mechanisms_ the OS uses to control **who can access what** inside the system.
    
    Example: Preventing one process from reading another process’s memory.
    
- **Security** = _defending against malicious attacks_ (both internal and external).
    
    Example: Stopping a hacker from logging into your system using stolen credentials.
    

Think of **protection** as the _locks inside your house_ (which rooms you can access) and **security** as the _front door, alarm system, and cameras_ that prevent burglars from getting in.

---

# 🛡️ 2. Protection in Operating Systems

Protection ensures **controlled access to resources** like CPU, memory, files, devices, and I/O.

### Examples of Protection Mechanisms:

1. **Memory Protection**
    - Hardware (MMU) ensures a process can only access memory in its **own address space**.
    - Prevents one process from corrupting another.
2. **CPU Protection**
    - A **timer** is used to ensure a process can’t hog the CPU forever.
    - The OS gets control back after each time slice (key for multiprogramming).
3. **I/O Device Protection**
    - Device registers are privileged: only the kernel can access them.
    - Users must go through system calls (like `read()`, `write()`) so the OS can enforce rules.
4. **File Protection**
    - Access control lists (ACLs) define who can read, write, or execute a file.
    - Example: On UNIX, permissions are given as `rwx` for **owner, group, others**.

✅ **Goal of protection**: prevent misuse (accidental or intentional) and detect errors early to stop one faulty subsystem from corrupting others.

---

# 🔒 3. Security in Operating Systems

Even if protection works, systems can still be attacked. Security is about **defending against threats** like:

- **Viruses and worms** → malicious code that spreads or damages data.
- **Denial-of-Service (DoS)** → attackers consume all CPU, memory, or network bandwidth to block legitimate users.
- **Identity theft** → attackers steal login credentials to impersonate real users.
- **Theft of service** → unauthorized people use computing resources without permission.

Security builds on protection, but goes further with **authentication, encryption, intrusion detection, and monitoring**.

---

# 👤 4. User Identification (UIDs and Groups)

The OS must **distinguish between users** to enforce protection and security policies.

- Every user has a **user ID (UID)** (Linux/UNIX) or **security ID (SID)** (Windows).
- All processes spawned by that user inherit their UID.
- The UID is used by the OS to check file permissions, memory access, and process privileges.
- For convenience, UIDs are mapped to **usernames** (e.g., UID 1000 → `alice`).

### Groups

- Sometimes we control access by **groups of users** instead of individuals.
- Example: A research file may be **writable only by a “research-team” group**.
- Each user can belong to multiple groups, and each process carries both UID and GIDs.

---

# ⚡ 5. Privilege Escalation

Most of the time, normal users run with **limited permissions**. But sometimes they need **temporary elevated privileges**.

- **Example:** A user wants to change their password. That requires writing to `/etc/shadow` (restricted file in UNIX).

### Solutions:

- **UNIX/Linux → setuid**
    - If a program is marked with `setuid`, when run, it executes with the **UID of the file owner** instead of the caller.
    - Example: `passwd` command runs with root’s UID to modify system password files safely.
- **Windows → User Account Control (UAC)**
    - Prompts users to confirm actions requiring admin rights.

⚠️ Risk: If privilege escalation is poorly implemented, attackers can exploit it to gain **root/admin access**.

---

# 🧩 6. Why Protection & Security Are Critical

- **Without protection** → one buggy program could overwrite kernel memory or steal another program’s data.
- **Without security** → even with good internal protection, an attacker could log in as you and steal everything.

Both are necessary:

- Protection enforces _order inside the system_.
- Security guards against _bad actors outside and inside_.

---

# 🔭 7. Modern Challenges

- **Cloud & virtualization** → now the OS must protect not just processes, but also **virtual machines** running on the same physical host.
- **Distributed systems** → multiple replicas of data require secure synchronization.
- **Ransomware & advanced persistent threats** → require OS-level features like encrypted filesystems and real-time intrusion detection.

---

✅ **In summary:**

- **Protection** = controlled access to CPU, memory, I/O, and files (internal safeguards).
- **Security** = defending against attacks like malware, theft, and misuse (external safeguards).
- Both require strong **user identification (UIDs/GIDs/SIDs)** and sometimes **privilege escalation** for special operations.

---

Would you like me to **draw a diagram of the relationship** (Protection = internal barriers, Security = external shield, both relying on UIDs/GIDs + policies), so you can visually see how they fit together?

  

  

# Virtualization

Alright — let’s go deep into **Section 1.7: Virtualization**. This is a crucial concept because it fundamentally reshaped how computing resources are used, especially in **data centers, cloud computing, and software development**.

---

# 🚀 1. What is Virtualization?

At its core, **virtualization is about abstraction**.

It allows one physical machine (with a single CPU, memory, disk, network card, etc.) to be split into **multiple virtual machines (VMs)**, each of which _behaves like a real computer_.

- Each VM can run its own **operating system** (guest OS).
- The **virtual machine manager (VMM)** (also called a _hypervisor_) mediates access to hardware, ensuring that each VM is isolated and protected.

👉 To the user, it feels like they have multiple separate computers, even though all are running on the same physical hardware.

---

# ⚙️ 2. Virtualization vs. Emulation

Many people confuse **virtualization** with **emulation**, but they are different:

|Feature|Virtualization|Emulation|
|---|---|---|
|CPU architecture|Same (e.g., host and guest both x86)|Different (e.g., ARM app running on x86 machine)|
|Performance|Near-native (minimal overhead)|Much slower (every instruction is translated)|
|Example|VMware, VirtualBox, KVM, Hyper-V|QEMU (full emulation mode), Rosetta (Apple PowerPC → Intel)|

- **Emulation** = full hardware simulation, used when source and target CPUs differ.
- **Virtualization** = guest OS already supports the host CPU type, so the hypervisor just shares resources.

---

# 🏛️ 3. Types of Virtualization

There are different ways virtualization is implemented, depending on where the **VMM (hypervisor)** runs.

### (a) Type 1 Hypervisor (Bare Metal)

- Runs **directly on hardware**, without a host OS.
- Provides **all OS-like services** (scheduling, resource management).
- Used in data centers for efficiency.
- Examples: VMware ESXi, Citrix Xen, Microsoft Hyper-V (bare-metal mode), KVM.

### (b) Type 2 Hypervisor (Hosted)

- Runs **inside an existing OS** (the host).
- Acts like a normal application that manages VMs.
- Easier to use, but less efficient because there are two OS layers.
- Examples: VMware Workstation, Oracle VirtualBox, Parallels (on macOS).

👉 Type 1 = for **servers & cloud**.

👉 Type 2 = for **desktops & testing**.

---

# 🖥️ 4. How Virtualization Works

The **Virtual Machine Manager (VMM)** handles:

1. **CPU virtualization**
    - Each VM thinks it has its own CPU, but in reality, the VMM schedules VM execution on physical CPUs.
    - Uses _context switching_ similar to processes.
2. **Memory virtualization**
    - Each VM sees its own **virtual memory**.
    - The VMM maps guest memory requests to host physical memory, often using **shadow page tables** or **hardware-assisted virtualization (Intel VT-x, AMD-V)**.
3. **Storage virtualization**
    - VMs see a virtual disk (e.g., `disk.vmdk` file).
    - This is just a file or block device on the host, but to the VM it looks like a full disk.
4. **Network virtualization**
    - VMs can have their own IP addresses and connect as if they had real NICs.
    - The VMM uses **virtual switches** to connect VMs to real networks.
5. **Isolation & protection**
    - Each VM is sandboxed, so crashes or malware inside one VM should not affect others.

---

# 🏢 5. Why Virtualization is Important

### (a) For Developers

- Run multiple OSes (Linux, Windows, BSD, etc.) on one machine.
- Develop and test cross-platform software without needing separate computers.

### (b) For Enterprises & Cloud

- **Server consolidation**: Instead of 10 physical servers running at 10% utilization, you can run 10 VMs on 1 physical server at 90% utilization.
- **Isolation**: Customers in the cloud (AWS, Azure, GCP) run VMs securely on the same physical machine.
- **Live migration**: Move running VMs from one host to another without downtime (important for maintenance).

### (c) For End-Users

- Run Windows apps on macOS (via Parallels/VMware).
- Try Linux on Windows via VirtualBox.

---

# 📊 6. Virtualization vs. Containers

Modern trend: **containers** (e.g., Docker, Kubernetes).

|Virtual Machines|Containers|
|---|---|
|Full OS per VM|Share host OS kernel|
|Heavy (GBs of memory, slower startup)|Lightweight (MBs, start in seconds)|
|Strong isolation (hardware-level)|Weaker isolation (process-level)|
|Best for different OSes|Best for microservices, scalable apps|

👉 Virtualization gives **hardware-level isolation**.

👉 Containers give **fast, process-level isolation**.

Often, **both are used together** (e.g., Kubernetes running inside VMs on AWS).

---

# 🔐 7. Challenges in Virtualization

- **Overhead**: Although efficient, VMs add performance overhead compared to bare metal.
- **Security**: If the hypervisor is compromised, all VMs are compromised.
- **Resource allocation**: Balancing CPU, memory, and I/O among competing VMs is tricky.

---

# ✅ Summary

- **Virtualization = multiple virtual computers running on one real computer**.
- Uses a **VMM/Hypervisor** to manage CPU, memory, disk, and networking.
- **Emulation** is different — slower, used when architectures don’t match.
- Crucial for **cloud computing, server consolidation, software development, and OS compatibility**.
- Modern evolution: **containers** (lighter-weight alternative, often used with VMs).

---

Would you like me to also **draw a diagram of Type 1 vs Type 2 hypervisors**, so you have a clean visual model of how host OS, guest OS, and hardware fit together?

  

![[/image 1 2.png|image 1 2.png]]

  

  

Perfect — let’s go **deep into Section 1.8: Distributed Systems**. This is one of the most important ideas in modern computing because nearly everything today — cloud platforms, mobile apps, databases, even multiplayer games — is built on distributed systems.

---

# 🌍 1. What is a Distributed System?

A **distributed system** is:

- A **collection of physically separate computers** (nodes)
- Connected by a **network**
- Working together to provide **shared resources and services** to users.

From the user’s perspective:

👉 It looks like **one unified system**, even though it is spread across many machines.

### Goals of distributed systems:

- **Performance** → Parallel execution can speed up computation.
- **Scalability** → Add more machines to handle more load.
- **Reliability & fault tolerance** → If one node crashes, others keep working.
- **Data sharing** → Users at different locations can access the same data.

Example:

- Google Search runs on **millions of servers**, but to you it feels like one simple system.
- Netflix streams movies from servers worldwide, but you just open the app and hit “Play.”

---

# 🕸️ 2. Networking as the Foundation

Distributed systems **depend on networks**.

- A **network** = communication path between two or more systems.
- The **protocols** define _how_ the machines talk to each other.
- The most important one is **TCP/IP** → the foundation of the Internet.

Operating systems handle networking by:

- Providing **device drivers** for network adapters (Ethernet, WiFi, Bluetooth).
- Exposing **APIs** for communication (e.g., `sockets` in UNIX/Linux).
- Sometimes abstracting network access as if it were a **file system** (e.g., NFS — Network File System).

👉 Example:

- In **FTP** you explicitly connect and transfer files.
- In **NFS**, opening a remote file looks the same as opening a local file.

---

# 🏗️ 3. Types of Networks in Distributed Systems

Networks are classified based on **distance between nodes**:

1. **PAN (Personal Area Network)**
    - Very short range (a few feet/meters).
    - Examples: Bluetooth (phone ↔ headset), infrared, NFC.
2. **LAN (Local Area Network)**
    - Covers a room, building, or campus.
    - Very high speed (Ethernet, Wi-Fi).
    - Common in homes and offices.
3. **MAN (Metropolitan Area Network)**
    - Connects nodes across a city.
    - Example: city-wide Wi-Fi, cable networks.
4. **WAN (Wide Area Network)**
    - Connects across countries/continents.
    - Example: the **Internet**.

### Media for transmission:

- **Copper wires** (Ethernet cables).
- **Fiber optics** (fast, long distance).
- **Wireless** (Wi-Fi, cellular 5G, satellite, microwave).

---

# 🖥️ 4. Network OS vs. Distributed OS

Two main OS approaches to distributed systems:

### (a) **Network Operating System (NOS)**

- Each computer runs its own OS, operates **independently**.
- Provides features like **file sharing, message passing**.
- Computers are aware of each other, but loosely coupled.
- Example: Windows Server, UNIX with NFS.

👉 Analogy: **Roommates sharing a house** — everyone is independent, but they occasionally share resources (Wi-Fi, printer).

### (b) **Distributed Operating System (DOS)**

- Multiple computers work so closely together that it looks like **a single OS** manages all.
- Provides **single-system image** → Users don’t know where resources are located.
- Example: Google Spanner, Amoeba OS (research), modern cloud cluster management.

👉 Analogy: **Organs in a single body** — highly coordinated, not independent.

---

# 🔑 5. Advantages of Distributed Systems

- **Resource Sharing** → Printers, files, storage, databases across machines.
- **Scalability** → Add more machines to handle growth.
- **Fault Tolerance** → Redundancy ensures one node’s failure doesn’t crash everything.
- **Concurrency** → Multiple users access shared data simultaneously.
- **Geographic Distribution** → Access resources globally.

---

# ⚠️ 6. Challenges of Distributed Systems

Distributed systems are powerful but **hard to build** because of:

1. **Transparency** → Users should not need to know where resources are.
    - Location transparency, replication transparency, etc.
2. **Fault tolerance** → Machines fail, networks drop packets. System must continue smoothly.
3. **Concurrency control** → Many users may update the same data. Requires synchronization (locks, consensus protocols).
4. **Security** → Data moves across networks → vulnerable to interception, spoofing, denial-of-service.
5. **Performance variability** → Different nodes may have different speeds, loads, or failures.

---

# 📌 7. Real-World Examples

- **Internet** itself = world’s largest distributed system.
- **Cloud computing** → AWS, Azure, Google Cloud are massive distributed systems.
- **Databases** → Cassandra, MongoDB, Google Spanner are distributed databases.
- **File systems** → Hadoop HDFS, Google File System, NFS.
- **Apps** → WhatsApp, Facebook, YouTube are distributed systems (millions of servers).

---

# ✅ Summary

- **Distributed system = multiple computers acting like one system**.
- Built on **networks (LAN, WAN, PAN, MAN)** with protocols like TCP/IP.
- Two main OS models:
    - **Network OS** → loose coupling, explicit communication.
    - **Distributed OS** → tight coupling, single-system illusion.
- Benefits: **scalability, reliability, resource sharing**.
- Challenges: **fault tolerance, concurrency, transparency, security**.

---

Would you like me to also draw a **diagram showing the difference between Network OS and Distributed OS** (like two clusters, one independent and one unified), so it’s easier to visualize?