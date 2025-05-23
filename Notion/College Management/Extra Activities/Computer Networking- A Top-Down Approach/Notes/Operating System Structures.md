---
Created: 2024-02-15T18:10
Type: Lecture
Materials:
  - "[[Chapter_2.pdf]]"
Reviewed: true
---
  

An OS provides an environment for the execution of programs.

- User Interface
    - GUI
    - CLI
- Program Execution
    - The system must be able to load a program into memory, run it, and terminate it, either normally or abnormally.
- I/O Operations
- File-system manipulation
    - programs need to r/w write files and directories.
    - Permission management
- Communications
- Error detection
- Resource Allocation
- Logging
- Protection and security

# User and OS Interface

  

## Command Interpreters

- The main function of the command interpreter is to get and execute the next user-specified command. This can be done in two ways:
    - put the code in the command interpreter. In this case, the number of commands that can be given determines the size of the command interpreter, since each command requires its own implementing code.
    - Implement most commands through system programs. In this case, the command interpreter does not understand the command; it merely uses the command to load a file or program into memory.

  

### Graphical User Interface

- Users employ a mouse-based window-and-menu system characterized by a desktop metaphor.

  

## Touch-Screen Interface

- users interact by making gestures on the touch screen.

  

  

# System Calls

- provide an interface to the services made available by an OS. These are generally available as functions in C and C++.

  

  

# Application Programming Interfaces or API

- Frequently, systems execute thousands of system calls per second. Typically, application developers design programs according to an API.
- The API specifies a set of functions that are available to an application programmer, including the parameters that are passed to each function and the return values the programmer can expect.
- A programmer accesses an API via a library of code provided by the OS.
- Behind the scenes, the functions that make up an API typically invoke the actual system clals on behalf of the application programmer.
- Why use APIs than actual system calls?
    - Program portability
    - System calls complexity
- Another important factor is the run-time environment or RTE—the full suite of software needed to execute applications written in a given programming language, including its compilers or interpreters as well as other software, such as libraries and loaders.
    - The RTE provides a system-call interface that serves as the link to system calls made available by the OS.
    - The system-call interface intercepts function calls in the API and invokes the necessary system calls within the OS. Typically, a number is associated with each system call, and the system-call interface maintains a table indexed according to these numbers. The system call interface then invokes the intended system call in the OS kernel and returns the status of the system call.
    - Most of the details of the OS interface are hidden from the programmer by the API and are managed by the RTE.
    - Three general methods are used to pass parameters to the OS:
        1. Passing the parameters in registers. But there may be more parameters than registers.
        2. In these cases, the parameters are generally stored in a block, or table, in memory, and the address of the block is passed as a parameter in a register.
            1. If there are >5 parameters, the block method is used. If < 5, registers are used.
        3. Parameters can also be placed, or pushed, onto a stack by the program and popped off the stack by the OS. Some operating systems prefer the block or stack method as those approaches do not limit the number or length of parameters being passed.

  

# Types of System Calls

  

## Process Control Calls

- A running program needs to be able to halt its execution either normally or abnormally. If a system is made to terminate the currently running program abnormally, or if the program runs into a problem and causes an error trap, a dump of memory is sometimes taken.
- Under either normal or abnormal circumstances, the OS must transfer control to the invoking command interpreter. The command interpreter simply then continues to read the next command. In a GUI system, a pop-up window might alert the user to the error.
- Error levels can also be defined for error prioritization and classification.
- A process executing one program may want to load() and execute() another program. But where to return control when the loaded program terminates?
    - If returned to the existing program when the new program terminates, we must save the memory image of the existing program.
    - If both programs continue concurrently, we have created a new process to be multiprogrammed.
- If we create a new process, or perhaps even a set of processes, we should be able to control its execution. We may also want to terminate a process if needed.
- Quite often, two or more processes may share data. To ensure the integrity of the data being shared, operating systems often provide system calls allowing a process to lock shared data. Then, no other process can access teh data until the lock is released. Typically, such system calls include acquire_lock() and release_lock().

  

## File Management

- create() and delete() files. Either system calls requires the name of the file and perhaps some of the file’s attributes.
- Once the file is created, we need to open() and to use it. We may also read(), write(), or reposition().
- close() the file.
- We also need the same sets for dirs. We also need to be able to determine the values of various attributes and to set them. get_file_attributes() and set_file_attributes()

  

## Device Management

- A process may need several resources to execute. If the resources are available, they can be granted, and control can be returned to the user process. Otherwise, the process will have to wait until sufficient resources are available.
- The various resources controlled by the OS can be thought of as devices. They can be physical or virtual devices.
- A system with multiple users may require to first request() a device, to ensure exclusive use of it. Then release() it. Other operating systems unmanaged access to devices. The hazard then is the potential for device connection and perhaps deadlock. Once the device has been requested and allocated, we can read(), write() and reposition() the device.

  

## Information Maintenance

- Purpose of transferring information between the user program and the OS. time(), date(),

  

  

## Communication

- Message-passing model
    - communicating processes exchange messages with one another to transfer information.
- Shared-memory model
    - processes use shared_memory_create() and shared_memory_attach() system calls to create and gain access to regions of memory owned by other processes.

  

## Protection

- provides a mechanism for controlling access to the resources provided by a computer system.

  

  

# System Services

- File management
- Status information
- File modification
- Programming-language support
- Program loading and execution
- Communications
- Background services

  

  

# Linkers and Loaders

Usually, a program resides on disk as a binary executable file. To run on a CPU, the program must be brought into memory and placed in the context of a process.

Source files are compiled into object files that are designed to be loaded into a physical memory location, a format known as a relocatable object file. Next, the linker combines these relocatable object files into a single binary executable file. During the linking phase, other object files or libraries may be included as well, such as the standard C or math lib.

A loader is used to load the binary executable file into memory, where it is eligible to run on a CPU core. An activity associated with linking and loading is relocation, which assigns final addresses to the program parts and adjusts code and data in the program to match those addresses so that, for example, the code can call library functions and access its variables as it executes.

![[/Untitled 9.png|Untitled 9.png]]

For example, in a CLI on UNIX systems, ./main—the shell first creates a new process to run the program using the fork() system call. THe shell then invokes the loader with the exec() system call, passing exec() the name of the executable file. The loader then loads the specified program into memory using the address space of the newly created process. In a GUI, double-clicking the icon associated with an exe file invokes the loader using a similar mechanism.

In reality, most systems allow a program to dynamically link libraries as the program is loaded. Like through dynamically linked libraries or DLLs in Windows. The benefit of this approach is that it avoids linking and loading libraries that may end up not being used into an exe file. Instead, the lib is conditionally linked and is loaded if it is required during program run time.

Object files and exe files typically have standard formats that include the compiled machine code and a symbol table containing metadata about functiosn and variables that are referenced in the program. For UNIX and Linux, this is ELF or Exeuctable and Linkable Format. One piece of information in the ELF file for exe files is the program’s entry point, which contains the address of the first instruction to be executed when the program runs. Windows use the Portable Executable or PE format and macOS uses the Mach-O format.

  

  

  

# Why Applications are OS-Specific

Fundamentally, applications are compiled on one OS are not executable on other operating systems.

We can see that the problem is that the OS provides a unique set of system calls. System calls are part of the set of services provided by operating systems for use by applications.

An application can be made cross-platform in one of three ways:

1. The applications can be written in an interpreted language that has an interpreter available for multiple operating systems.
    1. The interpreter reads each line of the source program, executes equivalent instructions on the native instruction set, and calls native OS calls.
    2. Performance suffers relative to that for native applications, and the interpreter provides only a subset of each operating system’s features, possibly limiting the feature sets of the associated applications.
2. The application can be written in a language that includes a virtual machine containing the running application. The virtual machine is part of the language’s full RTE.
    1. Example is the JVM. It has an RTE that includes a loader, byte-code verifier, and other components that load the Java application into the JVM. This RTE has been ported, or developed, for many operating systems. It has the same disadvantages as the interpreters.
3. The applications developer can use a standard language or API in which the compiler generates binraries in a machine- and OS-specific language. The application must be ported to each OS in which it will run. This porting is time consuming and must be done with every version.

  

The general lack of application mobility makes cross-platform applications a challenging task.

At the application level, the libs provided with the OS contains APIs to provide features like GUI interfaces, and an application designed to call one set of APIs will not work on an OS that does not provide those APIs.

Other challenges are that:

- Each OS has a binary format for applications that dictates the layout of the header, instructions, and variables. Those components need to be at certain locations in specified structures within an executable file so the OS can open the file and load the application for proper execution.
- CPUs have varying instruction sets, and only applications containing the appropraite instructions can execute correctly.
- System calls vary among operating systems

  

# Operating-System Design and Implementation

- the first problem in designing a system is to define goals and specifications. At the highest level, the design of the system will be affected by the choide of hardware and type of system.
    
    - User goals
        - system should be convenient to use
        - Easy to learn and use
        - Reliable
        - Safe
        - Fast
    - System goals
        - The system should be easy to design, implement, and operate the system.
        - Flexible
        - Reliable
        - Error free
        - Efficient
    
    There is no unique solution to the problem of defining the requirements for an OS. Specifying and designing an OS is a highly creative task.
    
      
    

  

## Mechanisms and Policies

  

Mechanisms determine how to do something; policies determine what will be done.

The separation of policy and mechanism is important for flexibility. Policies are likely to change across places or over time. In the worst case, each change in policy would require a change in the underlying mechanism.

A general mechanism flexible enough to work across a range of policies is preferable. A change in policy would then require redefinition of only certain parameters of the system.

  

Microkernel-based operating systems take the separation of mechanism and policy to one extreme by implementing a basic set of primitive building blocks. These blocks are almost policy free, allowing more advanced mechanisms and policies to be added via user-created kernel modules or user programs themselves.

Policy decisions are important for all resource allocation. Whenever it is necessary to decide whether or not to allocate a resource, a poliyc decision must be made. Whenever the question is how rather than what, it is a mechanism that must be determined.

  

  

## Implementation

Once an OS is designed, it must be implemented.

  

Early operating systems were written in assembly language. Now, most are writtien in C or C++, with small amounts of the system written in assembly language.

Advantages of using a higher-level language:

- code can be written faster
- more compact
- easier to understand and debug
- easier to port

Disadvantages of using a higher-level language in OS development:

- reduced speed
- increased storage requirements

However, these disadvantages are usually negligible in modern hardware.

  

As is true in other systems, major performance improvements in operating systems are more likely to be the result of better data structures and algorithms than of excellent assembly-language code. In addition, although operating systems are large, only a small amount of the code is critical to high performance; the interrupt handlers, I/O manager, memory manager, and CPU scheduler are the most critical routines.

  

# Operating-System Structure

A system as large and complex as a modern OS must be engineered carefully if it is to function properly and be modified easily.

A common approach is to partition the task into small components, or modules, rather than have one single system.

- Each of these modules should be well-defined portion of the system, with carefully defined interfaces and functions.

![[/Untitled 1 3.png|Untitled 1 3.png]]

  

  

## Monolithic Structure

The simplest structure for organizing an OS is no structure at all. That is, place all of the functionality of the kernel into as single, static binary file that runs in a single address space.

An example is the UNIX system, which consists of two separable parts: the kernel and the system programs. The kernel is further separated into a series of interfaces and device drivers, which have been added and expanded.

The LInux OS is the same. It is based on UNIX and is structured similarly. Applications typically use the glibc standard C library when communicating with the system call interface to the kernel. The Linux kernel is monolithic in that it runs entirely in kernel mode in a single address space, but it does have a modular design that allows the kernel to be modified during run time.

Despite simplicity, monolithic kernels are still difficult to implement and extend.

Monolithic kernels do have a distinct performance advantage: there is very little overhead in the system-call interface and communication within the kernel is fast.