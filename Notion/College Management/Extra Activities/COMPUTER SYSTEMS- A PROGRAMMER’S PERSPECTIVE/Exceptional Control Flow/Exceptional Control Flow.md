  

From the time you first apply power to a processor until the time you shut if off, the program counter assumes a sequence of values

  

$a_0,a_1,...a_{n-1}$

  

where each $a_k$ is the address of some corresponding instruction $I_k$. Each trnsition from a_k to a_k+1 is called a control transfer. a sequence of such control transfers is called the flow of control, or control flow, of the processor.

  

typically abrupt changes where I_k+1 is not adjacent to I_k are caused by familiar program instruction such as jumps, calls, and returns. but systems must also be able to react to changes in system state that are not captured by internal program variables and not necessarily related to the execution of the program.

  

Modern systems react to these situations by making abrupt changes in the control flow. in general, we refer to these abrupt changes as exceptional control flow or ECF.

- understanding ecf will help understand important systems concepts
- understanding ecf will help understand how apps interact with the os.
- helps in understanding concurrency
- helps in understanding how software exceptions work

  

  

# Exceptions

- exceptions are a form of exceptional control flow that are implemented partly by the hardware and os
- an exception is an abrupt change in the control flow in response to some change in the processor’s state. the change in state is known as an event.
- a change in the processor’s state (an event) triggers an abrupt control transfer (an exception) from the application program to an exception handler. after it finishes processing, the handler either return control to the interrupted program or aborts.

![[/image 6.png|image 6.png]]

- when the processor detects that the event has occurred, it makes an indirect procedure call (the exception), through a jump table called an exception table, to an OS subroutine (the exception handler) that is specifically designed to process this particular kind of event. when the exception handler finishes, one can happen:
    - the handler returns control to the current instruction I_curr, the instruction that was executing when the event occurred
    - the handler returns control to I_next, the instruction that would have executed next.
    - the handler aborts the interrupted program.

![[/image 1 3.png|image 1 3.png]]

## Exception Handling

- each type of possible exception in a system is assigned a unique nonnegative integer exception number. some are assigned by the designers of the processor. other number are assigned by the os kernel designers (the memory-resident part of the os).
- at system boot time, the os allocates and initializes a jump table called an exception table, so that entry k contains the address of the handler for exception k.
- at run time, the processor detects that an event has occurred and determines the corresponding exception number k. the processor then triggers the exception by making an indirect procedure call, through entry k of the exception table, to the corresponding handler. the exception number is an index into the exception table, whose starting address is contained in a special cpu register called the exception table base register.

  

An exception is similar to a procedure call but with differences:

- as with a procedure call, the processor pushes a return address on the stack before branching to the handler. depending on the class of exception, the return address is the curr instruction or the next.
- processor also pushes additional processor state onto stack necessary to restart the interrupted programs when the handler returns. Ex. EFLAGS register containing the current condition codes
- when control is being transferred from a user program to the kernel, all of these items are pushed onto the kernel;s stack rather than onto the user’s stack.
- exception handlers run in kernel mode which means they have complete access to all system resources.

  

once the hardware triggers the exception, the rest is done in software by the exception handler. after the handler processes the event, it optionally returns to the interrupted program by executing a special ‘return from interrupt’ instruction, which pops the state back in to the processor’s control and data registers.

  

  

## Classes of Exceptions

![[/image 2 3.png|image 2 3.png]]

Asynchronous exceptions occur as a result of events in I/O devices. synchronous exceptions occur as a direct result of a executing an instruction.

### Interrupts

- occur asynchronously as a result of signals from I/O devices that are external to the processor. hardware interrupts are asynchronous in the sense that they are not caused by the execution of any particular instruction. exception handlers for hardware interrupts are often called **interrupt handlers**

![[/image 3 3.png|image 3 3.png]]

- after the current instruction finishes executing, the processor notices that the interrupt pin has gone high, reads the exception number from the system bus, and then calls the appropriate interruption handler.

  

The remaining classes of exceptions occur synchronously as a result of executing the current instruction.

  

### Traps and System Calls

- Traps are intentional exceptions that occur as a result of executing an instruction.
- The most important use is to provide a procedure-like interface between the user programs and the kernel, known as a system call.
- user programs often need to request services from the kernel such as reading a file (read), creating new processes (fork), and loading new programs (execve). to allow controlled access to such kernel services, processors provide a special syscall n instruction that user programs can execute when they want to request service n.
- executing the syscall instruction causes a trap to an exception handler that decodes the argument and calls the appropriate kernel routine.
- a system call is different from the regular function. Regular functions run in user mode, which restricts the type of instructions they can execute, and they access the same stack as the calling function. a system call runs in kernel mode, which allows it to execute privileged instructions and access a stack defined in the kernel.

  

### Faults

- Faults result from error conditions that a handler might be able to correct. when a fault occurs, the processor transfers control to the fault handler.
    - if the handler can correct the error condition, it returns control to the faulting instruction, thereby re-executing it.
    - otherwise, the handler returns to an abort routine in the kernel that terminates the application program that caused the fault.

  

### Aborts

- aborts result from unrecoverable fatal errors, typically hardware errors such as parity errors. abort handlers never return control to the application program.

![[/image 4 2.png|image 4 2.png]]

  

## Exceptions

- exceptions are the basic building blocks that allow the OS kernel to provide the notion of a process.
- when we run a program on a modern system, we are in the illusion that we have all the attention of the processor and memory.
- the classic definition of a process is an instance of a program in execution. each program in the system runs in the context of some process. the context consists of the state that the program needs to run correctly.

  

### Logical control flow

- a process provides each program with the illusion that it has exclusive use of the processor.
- if we use a debugger, a series of program counter values that corresponded exclusively to instructions can be seen. this sequence of PC values is known as a logical control flow, or simply logical flow.

![[/image 5 2.png|image 5 2.png]]

- processes take turns using the processor. each processor executes a portion of its flow and then is preempted (temporarily suspended) while other processes take their turns.

  

### Concurrent flows

- a logical flow whose execution overlaps in time with another flow is calkled a concurrent flow, and the two flows are said to run concurrently.
    - more precisely, flows x and y are concurrent with respect to each other if and only if X begins after Y begins and before Y finishes, or Y begins after X begins and before X finishes.
- the general phenomenon of multiple flows executing concurrently is known as concurrency. the notion of a process taking turns with other processes is also known as multitasking. each time period that a process executes a portion of its flow is called a time slice. thus, multitasking is also referred to as time slicing.
- a proper subset of concurrent flows are parallel flows. if two flows are running concurrently on different processor cores or computers, then we say that they are parallel flows, that they are running in parallel and have parallel execution.

  

### Private Address Space

- a process provides each program with its own private address space.
    - a byte of memory associated with a particular address in the space cannot be in general be read or writte by any other process.

  

### User and Kernel Modes

- in order for the OS to provide an airtight process abstraction, the processor must provide a mechanism that restricts the instructions that an application can execute as well as the portions of the address space that it can access.
- processors typically provide this with a mode bit in some control register. when the mode bit is set, the process is running in kernel mode (aka supervisor mode). a process running in kernel mode can execute any instruction in the instruction set and access any memory location in the system.
- when the mode bit is not set, the process is running in user mode. a process in user mode is not allowed to execute privileged instructions. it also can’t directly reference code or data in the kernel area of the address space. any such attempt results in a fatal protection fault. user programs must instead access kernel code and data indirectly via the system call interface.
- a process running application code is initially in user mode. the only way for a the process to change from user to kernel mode is via an exception. when the exception occurs, the control passes to the exception handler, the processor changes the mode from user to kernel mode. the handler runs in kernel mode. when it returns to the application code, the processor changes the mode from kernel to user.

  

  

  

# **🔵 Context Switching in Operating Systems**

## **🔹 What is a Context Switch?**

A **context switch** is the process where the CPU **switches from executing one process (or thread) to another**. It involves **saving the state of the current process** and **restoring the state of the next process** so that execution can resume seamlessly.

📌 **Why do context switches happen?**

✅ **Multitasking** – Switching between processes in a time-shared system.

✅ **Multi-threading** – Switching between threads within a process.

✅ **Interrupt Handling** – When hardware (e.g., keyboard, disk) needs attention.

✅ **Kernel Mode ↔ User Mode Switch** – When a process requests OS services (e.g., `read()`, `write()` syscalls).

---

## **🔹 Types of Context Switches**

There are three main types of context switches:

1️⃣ **Process Switch (Task Switch)**

- The OS switches from executing one process to another.
- Used in **preemptive multitasking** (e.g., when a process exhausts its time slice).

2️⃣ **Thread Switch**

- Switching between threads of the same process.
- Cheaper than a process switch (less state to save/restore).

3️⃣ **Mode Switch (User Mode ↔ Kernel Mode)**

- When a process makes a **system call**, causing a switch from **user mode** to **kernel mode**.
- **Not a full context switch** since the process remains the same.

---

## **🔹 How Does a Context Switch Work?**

The OS context switch involves these key steps:

1️⃣ **Save the state of the current process (PCB - Process Control Block):**

- Program Counter (PC) → Tracks where execution stopped.
- CPU Registers → Store values of ongoing computations.
- Stack Pointer (SP) → Points to the process's stack.
- Memory Mappings → Virtual memory info (page table, etc.).

2️⃣ **Choose the next process to run (Scheduler Decision):**

- The OS picks the next process from the **ready queue** based on scheduling policies (e.g., Round-Robin, Priority Scheduling).

3️⃣ **Restore the state of the new process:**

- Load the saved **program counter**, **registers**, and **stack pointer**.
- Update **page tables** (if switching processes).
- Resume execution of the new process.

---

## **🔹 When Does a Context Switch Happen?**

A context switch can occur due to:

|   |   |
|---|---|
|**Trigger**|**Description**|
|**Timer Interrupt**|The scheduler preempts the running process when its time slice expires.|
|**I/O Interrupt**|The CPU switches to another process while waiting for I/O to complete.|
|**System Call**|A process calls the kernel (e.g., `read()`, `write()`, `fork()`).|
|**Process Termination**|When a process exits, the OS switches to another.|
|**Explicit Yield**|A process voluntarily gives up the CPU (`sched_yield()`).|

---

## **🔹 Performance Cost of Context Switching**

🔴 **Context switches are expensive** because they involve:  
✅ **Saving/restoring CPU registers**

✅ **Updating the memory management unit (MMU)** (e.g., switching page tables)

✅ **Flushing TLB (Translation Lookaside Buffer) caches**

✅ **Switching kernel stacks (if switching between processes)**

A **high frequency of context switches** can degrade performance due to the overhead.

🔍 **Check Context Switch Rate on Linux**

```Plain
vmstat 1
```

Example output:

```Plain
procs -----------memory---------- ---swap-- -----io---- -system-- ------cpu-----
 r  b   swpd   free   buff  cache   si   so    bi    bo   in   cs us sy id wa st
 1  0      0  10000  50000 200000    0    0     1     1  500 1000 10  5 80  5  0
```

📌 `cs` (context switches per second) indicates the system's context-switching rate.

---

## **🔹 How to Reduce Context Switch Overhead?**

✔ **Use efficient scheduling algorithms** (avoid excessive preemptions).

✔ **Use fewer threads/processes** (reduces contention & switching overhead).

✔ **Batch syscalls & avoid frequent I/O requests**.

✔ **Use kernel threads instead of user threads** (to avoid unnecessary user-space switches).

✔ **Utilize processor affinity** (keep processes on the same CPU core to reduce cache invalidation).

---

## **🚀 Summary**

✔ **Context switching is essential for multitasking & multi-threading.**

✔ **It involves saving/restoring CPU state, memory mappings, and registers.**

✔ **Can be triggered by interrupts, syscalls, or scheduler decisions.**

✔ **Has performance overhead—excessive context switching slows down execution.**

✔ **Optimizing scheduling & reducing unnecessary context switches improves performance.**

Would you like a deeper dive into **scheduler algorithms** that minimize context switches? 🚀

  

  

# **🔵 Process Control in Operating Systems**

## **🔹 What is Process Control?**

**Process control** refers to the OS **managing, creating, scheduling, and terminating processes**. The OS ensures that processes share system resources efficiently while maintaining stability and security.

The **Operating System (OS) Kernel** is responsible for:  
✅ Creating & terminating processes  

✅ Scheduling processes for execution

✅ Managing process states and transitions

✅ Allocating CPU, memory, and I/O resources

✅ Handling inter-process communication (IPC)

---

## **🔹 Process Control Block (PCB)**

Each process is represented by a **Process Control Block (PCB)**, which stores essential information about the process.

📌 **What’s inside the PCB?**

|   |   |
|---|---|
|**Field**|**Description**|
|**Process ID (PID)**|Unique identifier assigned by the OS.|
|**Process State**|Running, Ready, Blocked, etc.|
|**Program Counter (PC)**|Address of the next instruction to execute.|
|**CPU Registers**|Stores registers like EAX, EBX (x86), RAX, RBX (x86-64).|
|**Memory Management Info**|Page tables, heap, stack details.|
|**Open File List**|Files the process has opened.|
|**Parent Process ID (PPID)**|The ID of the parent process.|
|**Priority**|Process scheduling priority.|
|**I/O Status**|Information about I/O requests.|

---

## **🔹 Process Lifecycle & State Transitions**

A process moves through multiple **states** during execution.

### **Process States**

|   |   |
|---|---|
|**State**|**Description**|
|**New**|Process is created but not yet scheduled.|
|**Ready**|Process is waiting in the ready queue for CPU time.|
|**Running**|The process is actively executing on the CPU.|
|**Blocked (Waiting)**|Process is waiting for I/O or an event to complete.|
|**Terminated (Zombie)**|Process has completed execution but still exists in PCB until the OS cleans it up.|

🔄 **Process State Transitions**

📌 A process **switches states** due to CPU scheduling, I/O requests, interrupts, or completion.

- **Ready → Running**: OS scheduler selects the process for execution.
- **Running → Blocked**: Process waits for I/O (e.g., `read()` syscall).
- **Running → Ready**: CPU time slice expires (preemption).
- **Blocked → Ready**: I/O completes, making the process ready to execute.
- **Running → Terminated**: Process finishes execution or is killed.

---

## **🔹 Process Creation & Termination**

Processes are created and managed by **system calls** like `fork()`, `exec()`, `wait()`, and `exit()`.

### **1️⃣ Process Creation**

Processes can create **child processes** using `fork()`.

Example in **Linux (C)**:

```C
\#include <stdio.h>
\#include <unistd.h>

int main() {
    pid_t pid = fork(); // Creates a child process

    if (pid == 0) {
        printf("Child Process (PID: %d)\n", getpid());
    } else {
        printf("Parent Process (PID: %d, Child PID: %d)\n", getpid(), pid);
    }

    return 0;
}
```

📌 **What happens in** `**fork()**`**?**

✅ Creates a **new child process** with a unique PID.

✅ The child **inherits the parent's memory space** (Copy-on-Write).

✅ Both parent and child execute the same code but with different PID values.

🔍 **Check running processes** using:

```Plain
ps -aux
```

or

```Plain
pstree -p
```

to see the process hierarchy.

---

### **2️⃣ Process Execution (**`**exec()**`**)**

A process can **replace itself** with a new program using `exec()`.

Example:

```C
\#include <stdio.h>
\#include <unistd.h>

int main() {
    printf("Before exec()\n");
    execl("/bin/ls", "ls", "-l", NULL); // Replaces current process with `ls`
    printf("This will not execute!\n");
    return 0;
}
```

📌 **What happens in** `**exec()**`**?**

✅ The current process is replaced with `/bin/ls`.

✅ No new process is created (unlike `fork()`).

✅ The **PID remains the same**, but the process runs new code.

---

### **3️⃣ Process Termination (**`**exit()**` **&** `**wait()**`**)**

A process **terminates** when:

- It **completes execution** (`return 0;` in `main()`).
- It **calls** `**exit()**` explicitly.
- The OS **kills it** (`kill -9 <PID>`).

Parent processes **wait** for children to finish using `wait()`.  
Example:  

```C
\#include <stdio.h>
\#include <unistd.h>
\#include <sys/wait.h>

int main() {
    pid_t pid = fork();

    if (pid == 0) {
        printf("Child process running...\n");
        sleep(2);
        printf("Child exiting.\n");
    } else {
        wait(NULL); // Parent waits for the child to exit
        printf("Parent continues after child exits.\n");
    }

    return 0;
}
```

📌 **Why use** `**wait()**`**?**

✅ Prevents **zombie processes** (terminated but still in PCB).

✅ Synchronizes parent & child execution.

---

## **🔹 Process Scheduling (CPU Scheduling)**

📌 The OS decides **which process gets CPU time** using scheduling algorithms.

### **Common CPU Scheduling Algorithms**

|   |   |   |
|---|---|---|
|**Algorithm**|**Description**|**Best For**|
|**FCFS (First Come, First Serve)**|Runs processes in arrival order.|Batch systems.|
|**SJF (Shortest Job First)**|Runs shortest processes first.|CPU-bound tasks.|
|**Round Robin (RR)**|Each process gets a time slice (quantum).|Multitasking OS.|
|**Priority Scheduling**|Higher-priority processes run first.|Real-time systems.|
|**Multilevel Queue Scheduling**|Separate queues for foreground & background tasks.|Interactive & batch processes.|

🔍 **Check process scheduling in Linux**:

```Plain
ps -eo pid,comm,pri,stat
```

📌 `PRI` = Priority, `STAT` = Process state.

---

## **🔹 Inter-Process Communication (IPC)**

Processes need to **communicate** when working together.

### **Common IPC Mechanisms**

|   |   |
|---|---|
|**IPC Mechanism**|**Description**|
|**Pipes (`|`)**|
|**Message Queues**|OS-managed queues for message passing.|
|**Shared Memory**|Fastest IPC method (shared memory segment).|
|**Sockets**|Used for network communication (TCP/IP).|
|**Signals**|Processes send/receive signals (e.g., `SIGKILL`).|

Example of using **pipes** in Linux:

```Plain
ls | grep ".c"
```

Here, `ls` writes output to the **pipe**, and `grep` reads from it.

---

## **🚀 Summary**

✔ **Process control** manages execution, scheduling, and termination.

✔ Each process has a **PCB** storing state info.

✔ **Process states**: New, Ready, Running, Blocked, Terminated.

✔ `fork()` creates a child process, `exec()` runs a new program, `wait()` prevents zombies.

✔ **Process scheduling** (Round Robin, Priority, etc.) decides CPU time allocation.

✔ **IPC (pipes, shared memory, signals, etc.)** allows processes to communicate.

Would you like a **deep dive into Linux kernel process scheduling**? 🚀

  

  

# **🔵 Forks, Child Processes, Reaping,** `**wait()**`**, and** `**sleep()**` **in Process Management**

## **🔹 What is** `**fork()**`**?**

`fork()` is a system call used in Unix/Linux to create a **new child process**. The new process is an **exact copy of the parent process** but has its **own unique process ID (PID)**.

📌 **Key facts about** `**fork()**`

✅ **Creates a new process (child) from an existing one (parent)**.

✅ The **child gets a duplicate memory space** (Copy-on-Write optimization).

✅ **Parent & child execute the same code** but follow different execution paths.

✅ Both processes **continue execution from the same point after** `**fork()**`.

✅ The return value **determines whether the process is the parent or child**:

- `**fork()**` **returns** `**0**` **to the child process.**
- `**fork()**` **returns the child's PID to the parent process.**
- **If** `**fork()**` **fails, it returns** `**1**` **(error).**

---

## **🔹 Example of** `**fork()**` **in C**

```C
\#include <stdio.h>
\#include <unistd.h>

int main() {
    pid_t pid = fork(); // Create child process

    if (pid == 0) {  // Child process
        printf("Child Process (PID: %d, Parent PID: %d)\n", getpid(), getppid());
    } else {  // Parent process
        printf("Parent Process (PID: %d, Child PID: %d)\n", getpid(), pid);
    }

    return 0;
}
```

### **🔍 Expected Output (Process IDs will vary)**

```Plain
Parent Process (PID: 1234, Child PID: 1235)
Child Process (PID: 1235, Parent PID: 1234)
```

📌 **Order of execution is unpredictable** because the OS schedules processes independently.

---

## **🔹 What Happens After** `**fork()**`**?**

1️⃣ The **parent process** continues execution normally.

2️⃣ The **child process starts executing the same code** but gets `0` as the return value from `fork()`.

3️⃣ Both processes can **run independently** and execute different logic.

---

## **🔹 Creating Multiple Child Processes**

You can call `fork()` multiple times to create multiple processes.

```C
\#include <stdio.h>
\#include <unistd.h>

int main() {
    fork();
    fork();
    printf("Process PID: %d\n", getpid()); // This line executes in all created processes
    return 0;
}
```

### **🔍 How many processes are created?**

Each `fork()` **doubles** the number of processes.

- 1st `fork()` → 2 processes (Parent + 1 Child)
- 2nd `fork()` → Each process forks again → 4 total processes

Expected Output (Order may vary):

```Plain
Process PID: 1001
Process PID: 1002
Process PID: 1003
Process PID: 1004
```

📌 **Rule: If** `**fork()**` **is called** `**n**` **times, it creates** `**2^n**` **processes.**

---

## **🔹 Orphan & Zombie Processes**

### **🔵 Orphan Processes**

An **orphan process** is a child process **whose parent has terminated**, but the child keeps running.

The **init/systemd** process (PID = 1) **adopts orphan processes**.

Example:

```C
\#include <stdio.h>
\#include <unistd.h>

int main() {
    pid_t pid = fork();

    if (pid > 0) {  // Parent process
        printf("Parent Process exiting...\n");
    } else if (pid == 0) {  // Child process
        sleep(5); // Child runs after parent exits
        printf("Child Process (Orphan) running. Parent PID: %d\n", getppid());
    }

    return 0;
}
```

**Output (5 sec delay before child's message):**

```Plain
Parent Process exiting...
Child Process (Orphan) running. Parent PID: 1
```

📌 **Notice:** The child’s new parent is `init` (PID 1).

---

### **🔴 Zombie Processes**

A **zombie process** is a child process that **has finished execution but is not removed from the process table** because the parent **did not call** `**wait()**`.

**How do zombie processes happen?**

- A child process exits.
- The parent **does not call** `**wait()**` to read the child’s exit status.
- The OS **keeps the child’s entry in the process table** (waiting for the parent to collect the status).

Example:

```C
\#include <stdio.h>
\#include <unistd.h>

int main() {
    pid_t pid = fork();

    if (pid == 0) {  // Child process
        printf("Child process exiting...\n");
    } else {  // Parent process
        sleep(10); // Parent does NOT call wait()
        printf("Parent process finishing...\n");
    }

    return 0;
}
```

Run `ps aux | grep Z` to check for zombie processes.

---

## **🔹 Reaping Child Processes: Using** `**wait()**`

To **prevent zombie processes**, the parent process must **"reap"** the child by calling `wait()`.

```C
\#include <stdio.h>
\#include <sys/wait.h>
\#include <unistd.h>

int main() {
    pid_t pid = fork();

    if (pid == 0) {  // Child process
        printf("Child Process Running...\n");
        sleep(2);
        printf("Child Process Exiting.\n");
    } else {  // Parent process
        wait(NULL); // Wait for the child to finish
        printf("Parent Process Continues After Child Exits.\n");
    }

    return 0;
}
```

### **🔍 Output (2 sec delay before parent prints last message)**

```Plain
Child Process Running...
Child Process Exiting.
Parent Process Continues After Child Exits.
```

📌 **What does** `**wait()**` **do?**

✅ Prevents **zombie processes** by cleaning up child resources.

✅ **Blocks** the parent **until the child exits**.

---

## **🔹 Non-Blocking** `**waitpid()**`

Instead of blocking, `waitpid()` can be used to **check for child process status**.

```C
\#include <stdio.h>
\#include <sys/wait.h>
\#include <unistd.h>

int main() {
    pid_t pid = fork();

    if (pid == 0) {  // Child process
        sleep(3);
        printf("Child Process Exiting.\n");
    } else {  // Parent process
        printf("Parent waiting for child...\n");
        waitpid(pid, NULL, 0); // Non-blocking wait
        printf("Parent Process Continues.\n");
    }

    return 0;
}
```

📌 `waitpid()` allows **waiting for a specific child process**.

---

## **🔹 Using** `**sleep()**`

The `sleep()` function **pauses a process for a given number of seconds**.

```C
\#include <stdio.h>
\#include <unistd.h>

int main() {
    printf("Sleeping for 5 seconds...\n");
    sleep(5);
    printf("Woke up!\n");
    return 0;
}
```

📌 **Use cases:**

✅ Simulating delays

✅ Waiting for I/O

✅ Synchronization

---

## **🚀 Summary**

✔ `fork()` creates a **child process** that runs independently.

✔ Child & parent **execute the same code** but follow different execution paths.

✔ **Orphan processes** are adopted by `init/systemd`.

✔ **Zombie processes** happen when the parent **doesn't call** `**wait()**`.

✔ `wait()` prevents **zombies** by cleaning up child processes.

✔ `sleep()` pauses execution for a given time.

Would you like an explanation of **inter-process communication (IPC)** next? 🚀

  

  

Great observation! But no, `fork()` **does not cause infinite recursion** because it doesn't actually **call** `**main()**` **again**—it just creates a **copy** of the process at the point where `fork()` was executed.

### **🔍 Why Doesn't** `**fork()**` **Cause Infinite Recursion?**

1. **No Recursive Function Call**:
    - `fork()` **does not call** `**main()**` **again**.
    - It simply **duplicates** the process **at that exact execution point**.
2. **Each Process Runs Independently**:
    - After `fork()`, both the **parent and child continue executing** from the same instruction, but **as separate processes**.
    - The **child does NOT restart** `**main()**`; it just continues execution **from the return point of** `**fork()**`.
3. **Processes are Not Re-entering** `**fork()**` **Unintentionally**:
    - The new child process only **inherits memory, variables, and state** but does not "re-execute" `fork()` unless explicitly coded to do so.
    - Each `fork()` creates **only one child** at a time.

---

### **🔍 Example: Recursive Fork (Which Would Actually Cause Exponential Growth)**

If you **explicitly** put `fork()` in a recursive function, **then** you can get infinite (or exponential) process creation.

```C
\#include <stdio.h>
\#include <unistd.h>

void recursive_fork(int depth) {
    if (depth > 0) {
        fork();
        recursive_fork(depth - 1);
    }
}

int main() {
    recursive_fork(3); // 2^3 = 8 processes will be created
    printf("PID: %d\n", getpid());
    return 0;
}
```

📌 This **actually causes exponential growth**:

- `recursive_fork(3)` → 2³ = 8 processes.
- `recursive_fork(10)` → 2¹⁰ = 1024 processes.

🚨 **Warning:** This will **quickly exhaust system resources** and crash your OS if you recurse too much.

---

### **🔍 The Correct Behavior of** `**fork()**`

If `fork()` were **actually calling** `**main()**` **again**, it would **never return** and would create an infinite loop. Instead, it **only duplicates** the current process and allows both processes to continue **independently**.

Would you like a deeper dive into how process memory (heap, stack, etc.) is handled after `fork()`? 🚀

  

  

Great question! When I say **"fork() only duplicates the process at that exact execution point,"** I mean that the `fork()` system call creates a **new child process that is an exact copy of the parent process at the moment** `**fork()**` **is called**.

### **🔹 What Happens When** `**fork()**` **is Called?**

- The **entire memory space of the parent process is copied** into the child.
- Both the **parent and child continue execution from the next instruction after** `**fork()**`.
- The child **does NOT restart** `**main()**`—it simply continues **from the same place where** `**fork()**` **happened**.

---

## **🔍 Example: Understanding Execution Flow**

```C
\#include <stdio.h>
\#include <unistd.h>

int main() {
    printf("Before fork (PID: %d)\n", getpid());

    pid_t pid = fork(); // Fork happens here

    if (pid == 0) {
        printf("Child Process (PID: %d, Parent PID: %d)\n", getpid(), getppid());
    } else {
        printf("Parent Process (PID: %d, Child PID: %d)\n", getpid(), pid);
    }

    printf("End of execution (PID: %d)\n", getpid());
    return 0;
}
```

---

### **🔍 Step-by-Step Breakdown**

1️⃣ **Parent process starts running**

```Plain
Before fork (PID: 1234)
```

2️⃣ `**fork()**` **is called** → The OS **creates a new process** (child) **that is an exact copy of the parent**.  
3️⃣ **Both processes continue execution from the same point (after** `**fork()**`**)**

- The **parent sees** `**fork()**` **return the child's PID**.
- The **child sees** `**fork()**` **return** `**0**`.
- **Both processes print their respective messages**:

```Plain
Parent Process (PID: 1234, Child PID: 1235)
Child Process (PID: 1235, Parent PID: 1234)
```

4️⃣ **Both processes continue to the next instruction independently**:

```Plain
End of execution (PID: 1234)
End of execution (PID: 1235)
```

📌 **Notice** that both the parent and child reach the last `printf()` statement **independently**.

---

### **🔍 Key Takeaways**

✅ **The child process does NOT restart** `**main()**`—it continues from the point where `fork()` was called.

✅ Both parent and child **execute the same instructions after** `**fork()**`, but independently.

✅ The return value of `fork()` tells us **which process is which**:

- **Parent** gets the child's PID.
- **Child** gets `0`.

Would you like me to illustrate this with a **process diagram**? 🚀

  

  

we can also notice that both parent and child print their output on the screen. the reason is that the child inherits all of the parent’s open files.

  

When a process terminates for any reason, the kernel does not remove it from the system immediately. the process is kept around in a terminated state until it is reaped by its parent. when the parent reaps the terminated child, the kernel passes the child’s exit status to the parent and then discards the terminated process, at which point it ceases to exist. a terminated process that has not yet been reaped is called a zombie.

when a parent process terminates, the kernel arranges for the init process to become the adopted parent of any orphaned children. THe init procss, which has a PID of 1, is created by the kernel during system start-up, never terminates, and is the ancestor of every process.

  

### **🔹** `**execve()**`**: Replacing a Process's Memory Image**

`execve()` is a **system call** that **replaces the current process's memory with a new program**. Unlike `fork()`, which creates a copy of the process, `execve()` **completely replaces** the calling process with a new executable.

This means:  
✅ The process ID (**PID**) remains the same.

✅ The previous code, stack, heap, and data are **completely overwritten**.

✅ Only the file descriptors (unless marked `FD_CLOEXEC`) and some environment settings remain.

---

### **🔹 Syntax of** `**execve()**`

```C
int execve(const char *pathname, char *const argv[], char *const envp[]);
```

- `pathname`: The **absolute path** to the executable file.
- `argv[]`: Array of arguments (similar to `argv` in `main()`).
    - `argv[0]` is conventionally the **program name**.
    - The array **must be NULL-terminated**.
- `envp[]`: Array of **environment variables** (also NULL-terminated).

---

### **🔹 Example: Using** `**execve()**`

Let's say we have a program `/bin/ls`, and we want our process to **replace itself** with `ls -l`.

```C
\#include <stdio.h>
\#include <unistd.h>

int main() {
    char *args[] = {"/bin/ls", "-l", NULL};  // Command-line arguments
    char *envp[] = {NULL};  // Use the current environment

    printf("Before execve()\n");

    if (execve("/bin/ls", args, envp) == -1) {
        perror("execve failed");
    }

    printf("This will never execute!\n");  // Never runs if execve succeeds
    return 0;
}
```

### **🔍 What Happens?**

1️⃣ The `execve()` call replaces **this program** with `/bin/ls`.

2️⃣ `ls -l` executes **as if it was started normally**.

3️⃣ **The original process code never resumes execution** (unless `execve()` fails).

4️⃣ The `printf("This will never execute!")` **is never reached** if `execve()` succeeds.

---

### **🔹 Combining** `**fork()**` **and** `**execve()**`

Since `execve()` **replaces the current process**, we often use it with `fork()` to start a **new** process:

```C
\#include <stdio.h>
\#include <unistd.h>
\#include <sys/types.h>

int main() {
    pid_t pid = fork();

    if (pid == 0) {  // Child process
        char *args[] = {"/bin/ls", "-l", NULL};
        execve("/bin/ls", args, NULL);
        perror("execve failed");  // Only runs if execve() fails
    } else {
        printf("Parent process: PID=%d\n", getpid());
    }

    return 0;
}
```

📌 **Here’s what happens**:

- The **parent process continues** running independently.
- The **child process calls** `**execve()**` and replaces itself with `ls -l`.
- The parent **does NOT get replaced**—only the child does.

---

### **🔹 What If** `**execve()**` **Fails?**

If the specified executable **does not exist or is not executable**, `execve()` **returns -1** and sets `errno`.

Example failure:

```C
execve("/bin/does_not_exist", args, NULL); // Will return -1
```

📌 Always **check for errors using** `**perror()**` **or** `**strerror(errno)**`.

---

### **🔹 Key Takeaways**

✅ `**execve()**` **completely replaces the process image**—execution never returns if successful.

✅ **Often used with** `**fork()**` to create new processes while keeping the parent running.

✅ **PID remains unchanged**—only the program code and memory are replaced.

✅ **If** `**execve()**` **fails, execution resumes after it**, so always check for errors.

Would you like me to explain **other exec-family functions** like `execlp()` and `execvp()`? 🚀

  

  

### **🔹 Signals in UNIX-like Systems**

A **signal** is an **asynchronous notification** sent to a process to **notify it of an event**. Signals are used for **process control, interprocess communication, and handling exceptions** (e.g., segmentation faults, termination requests).

---

## **🔹 How Signals Work**

1. **A process receives a signal**.
2. The process can **either handle the signal** (using a signal handler) **or take the default action**.
3. If not handled, the kernel applies the **default behavior** for that signal (e.g., terminating the process).

---

## **🔹 Common Signals**

|   |   |   |   |
|---|---|---|---|
|Signal|Number|Default Action|Description|
|`SIGINT`|2|Terminate|Sent when the user presses `Ctrl+C`|
|`SIGTERM`|15|Terminate|Request to terminate a process gracefully|
|`SIGKILL`|9|Terminate|Immediately kill a process (cannot be caught)|
|`SIGSTOP`|19|Stop|Stop process execution (`Ctrl+Z`)|
|`SIGCONT`|18|Continue|Resume a stopped process|
|`SIGSEGV`|11|Terminate & Dump|Segmentation fault (invalid memory access)|
|`SIGHUP`|1|Terminate|Terminal hangup (used to reload configurations)|

---

## **🔹 Sending Signals**

### **1️⃣ From the Terminal**

- `Ctrl+C` → Sends `SIGINT` to the foreground process.
- `Ctrl+Z` → Sends `SIGSTOP` to the foreground process.
- `kill -9 <pid>` → Sends `SIGKILL` to terminate a process forcefully.

### **2️⃣ Using the** `**kill()**` **System Call**

A process can send signals programmatically:

```C
\#include <signal.h>
\#include <stdio.h>
\#include <unistd.h>

int main() {
    pid_t pid = 12345;  // Replace with target process ID
    kill(pid, SIGTERM); // Send SIGTERM to process 12345
    return 0;
}
```

---

## **🔹 Handling Signals with** `**signal()**`

A process can **catch and handle** certain signals instead of using the default action.

### **Example: Handling** `**SIGINT**` **(**`**Ctrl+C**`**)**

```C
\#include <stdio.h>
\#include <signal.h>
\#include <unistd.h>

void handle_sigint(int sig) {
    printf("Caught signal %d (SIGINT). Ignoring Ctrl+C.\n", sig);
}

int main() {
    signal(SIGINT, handle_sigint);  // Register signal handler

    while (1) {
        printf("Running... Press Ctrl+C\n");
        sleep(1);
    }

    return 0;
}
```

📌 **Now, pressing** `**Ctrl+C**` **won’t terminate the process**—instead, it prints a message.

---

## **🔹 Using** `**sigaction()**` **(More Reliable than** `**signal()**`**)**

The `sigaction()` system call provides **more control** than `signal()`.

```C
\#include <stdio.h>
\#include <signal.h>
\#include <unistd.h>

void handle_sigterm(int sig) {
    printf("Received SIGTERM! Cleaning up...\n");
}

int main() {
    struct sigaction sa;
    sa.sa_handler = handle_sigterm;
    sigemptyset(&sa.sa_mask);
    sa.sa_flags = 0;

    sigaction(SIGTERM, &sa, NULL);  // Register handler

    while (1) {
        printf("Running... Send SIGTERM to stop.\n");
        sleep(2);
    }

    return 0;
}
```

📌 **More powerful than** `**signal()**` **because it allows blocking other signals while handling one.**

---

## **🔹 Ignoring Signals**

You can **ignore** signals by setting the handler to `SIG_IGN`:

```C
signal(SIGINT, SIG_IGN); // Ignore Ctrl+C
```

---

## **🔹 Restoring Default Behavior**

Use `SIG_DFL` to restore a signal’s default behavior:

```C
signal(SIGINT, SIG_DFL); // Restore default behavior for SIGINT
```

---

## **🔹 Signals That Cannot Be Caught or Ignored**

🚨 `**SIGKILL**` **and** `**SIGSTOP**` **cannot be caught, blocked, or ignored!**

The kernel forces these signals to be obeyed.

---

## **🔹 Summary**

✅ **Signals notify processes about events (e.g., termination, segmentation faults).**

✅ **Signals can be handled using** `**signal()**` **or** `**sigaction()**` **to prevent default behavior.**

✅ **Some signals (like** `**SIGKILL**`**) cannot be caught or ignored.**

✅ **Processes can send signals using** `**kill()**` **or** `**raise()**`**.**

Would you like me to dive deeper into **real-time signals or signal masking**? 🚀