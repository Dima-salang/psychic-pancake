---
Created: 2024-03-16T00:49
Type: Lecture
Materials:
  - "[[Chapter_1.pdf]]"
Reviewed: true
---
Overview

- An operating system acts as an intermediary between the user of a computer and the computer hardware. The purpose of an operating system is to provide an environment in which a use can execute programs in a convenient and efficient manner.
- An OS is software that manages the computer hardware. The hardware must provide appropriate mechanisms to ensure the correct operation of the computer system and prevent programs from interfering with the proper operation of the system
- Internally, operating systems vary greatly in their makeup. The design of a new OS is a major task, and it is important that the goals of the system be well defined before the design begins.
- Because an OS is large and complex, it must be created piece by piece. Each of these pieces should be a well-delineated portion of the system, with carefully defined inputs, outputs, and functions.

1.1 What Operating Systems Do

- A computer system can be divided roughly into four components:
    - Hardware
        - CPU, memory, and I/O devices - provides the basic computing resources for the system
    - OS
        - Controls the hardware and coordinates its use among the various application programs for the various users
- Application programs
    - Define the ways in which the hardware are used to solve users' computing problems
- User
- We can also view a computer system as consisting of hardware, software, and data. The OS provides the means for proper use of these resources in the operation of the computer system. An OS is similar to a government. Like a government, it performs no useful function by itself. It simply provides an environment within which other programs can do useful work.

Defining Operating Systems

- Computing tarted as an experiment to determine what could be done and quickly moved to fixed-purpose systems for military uses, such as code breaking. Those early computer evolved into general-purpose mainframes, and that's when operating systems were born.
- In general, we have no completely adequate definition of an OS. **Operating systems exist because they offer a reasonable way to solve the problem of creating a usable computing system. The fundamental goal of computer systems is to execute programs and to make solving user problems easier. Computer hardware is constructed toward this goal.** Since bare hardware alone is not particularly easy to use, application programs are developed. These programs require certain common operations , such as controlling I/O devices. **The common functions of controlling and allocating resources are then brought together into one piece of software: the OS.**
- In addition, we have no universally accepted definitoin of what is part of the OS. **A more common definition is that the OS is the one program running at all times on the computer - usually called the kernel.** Along with the kernel, there are two other types of programs: system programs, which are associated with the OS but are not necessarily part of the kernel, and application programs, which include all programs not associated with the operation of the system.
- If we look at operating systems for mobile devices, we see that the number of features constituting the OS is increasing. Mobile operating systems often include not only a core kernel but also middleware - a set of software frameworks that provide additional services to application developers.
- In summary, the Os includes the always-running kernel, middleware frameworks that ease application development and provide features, and system programs that aid in managing the system while it is running.

Computer System Organization

- A modern general-purpose computer system consists of one or more CPUs and a number of **device controller**s connected through a common bus that provides access between components and shared memory. **Each device controller is in charge of a specific type of device.** Depending on the controller, more than one device may be attached. **A device controller maintains some local buffer storage and a set of special-purpose registers. The device controller is responsible for moving the data between the peripheral devices that it controls and its local buffer storage.**
- **Typically, operating systems have a device driver for each device controller. This device driver understands the device controller and provides the rest of the OS with a uniform interface to the device.** The CPU and the device controllers can execute in parallel, competing for memory cycles. To ensure orderly access to the shared memory, a memory controller synchronizes access to the memory.
- Interrupts
    
    - Consider a typical computer operation: a program performing I/O. To start an I/O operation, the device driver loads the appropriate registers in the device controller. The device controller, in turn, examines the contents of these registers to determine what action to take (such as read a character from the keyboard). The controller starts the transfer of data from the device to its local buffer. Once the transfer of data is complete, the device controller informs the device driver that it has finished its operation. The device driver then gives control to other parts of the OS, possibly returning the data or a pointer to the data if the operation was a read. But how does the controller inform the device driver that it has finished its operation? This is accomplished via interrupts.
    - Hardware may trigger an interrupt at any time by sending a signal to the CPU, usually by way of the system bus. (There may be many buses within a computer system, but the system bus is the main communications path between the major components.)
    - When the CPU is interrupted, it stops what it is doing and immediately transfers execution to a fixed location. The fixed location usually contains the starting address where the service routine for the interrupt is located. The interrupt service routine executes; on completion, the CPU resumes the interrupted computation.
    - The interrupt must transfer control to the appropriate interrupt service routine. The straightforward method for managing this transfer would be to invoke a generic routine to examine the interrupt information. The routine, in turn, would call the interrupt-specific handler. However, interrupt must be handled quickly as they occur very frequently. A table of pointers to interrupt routines can be used instead to provide the necessary speed.
    - The interrupt routine is called indirectly through the table, with no intermediate routine needed. Generally, the table of pointers is stored in low memory (the first hundred or so locations). These locations hold the addresses of the interrupt service routines for the variousa devices. This array, or interrupt vector, of addresses is then indexed by a unqiue number, given with the interrupt request, to provide the address of the interrupt service routine for the interrupting device.
    
      
    
    ### Interrupt Implementation
    
    - THe CPU hardware has a wire called the interrupt-request line that the CPU senses after executing every instruction. When the CPU detects that a controller has asserted a signal on the interrupt-request line, it reads the interrupt number and jumpts to the interrupt-handler routine by using that interrupt number as an index into the interrupt vector
    - It then starts execution at the address associated with that index. The interrupt handler saves any state it will be changing during its operation, determines the cause of the interrupt, performs the necessary instruction to return the CPU to the execution state prior to the interrupt.
    - **We say that the device controller raises an interrupt by asserting a signal on the interrupt request line, the CPU catches the interrupt and dispatches it to the interrupt handler, and the handler clears the interrupt by servicing the device.**
    
    For modern operating systems, we need more sophisticated interrupt handling features.
    
    1. We need the ability to defer interrupt handling during critical processing.
    2. We need an efficient way to dispatch to the proper interrupt handler for a device
    3. We need multilevel interrupt, so that the OS can distinguish between high- and low-priority interrupts.

  

Most CPUs have two interrupt request lines. One is the **nonmaskable interrupt** which is reserved for events such as unrecoverable memory errors. The second interrupt line is **maskable**: it can be turned off by the CPU before the execution of critical instruction sequences that must not be interrupted. The maskable interrupt is used by the device controllers to request service.

In practice, computers have more devices than they have address elements in the interrupt vector. A common way to solve this is to use interrupt chaining, in which each element in the interrupt vector points to the head of a list of interrupt handlers. This structure is a compromise between the overhead of a huge interrupt table and the inefficiency of dispatching to a single interrupt handler.

THe interrupt mechanism also implements a system of interrupt priority levels. These levels enable the CPU to defer the handling of low-priority interrupts without masking all interrupts and makes it possible for a high-priority interrupt to preempt the execution of a low-priority interrupt.

  

  

# Storage Structure

The CPU can load instructions only from memory, so any programs must first be loaded into memory to run. General-purpose programs run most of their programs from **RAM** or main memory. Main memory commonly is implemented in a semiconductor technology called DRAM.

- The first program to run on computer power-on is a **bootstrap program**, which then loads the OS. Since RAM is **volatile** - it loses its contents. Therefore, the bootstrap program is stored in **electrically erasable programmable read-only memory or EEPROM** and other forms of **firmware**—storage that is infrequently written to and is nonvolatile. EEPROM can be changed but cannot be changed infrequently. In addition, it is low speed, and so it contains mostly static programs and data that aren’t frequently used.

All forms of memory provide an array of bytes. Each byte has its own address. Interaction is achieved through a sequence of load or store instructions to specific memory addresses.

- The load instruction moves a byte or word from memory to an internal register within the CPU, whereas the store instruction moves the content of a register to main memory.
- Aside from explicit loads and stores, the CPU automatically loads instructions from main memory for execution from the location stored in the program counter.

A typical instruction-execution cycle as executed on a system with Von Neumann architecture, first fetches an instruction from memory and stores that instruction in the instruction register. The instruction is then decoded and may cause operands to be fetched from memory and stored in some internal register. After the instruction on the operands, the result may be stored back in memory. Notica that the memory unit sees only a stream of memory addresses. We are interested only in the sequence of memory addresses generated by the running program.

  

Secondary storage is an extension of main memory. It can hold large quantities permanently.

- The most common secondary-storage devices are HDDs and nonvolatile memory or NVM devices.
- Most programs are stored in secondary storage until they are loaded into memory. Many programs then use secondary storage as both the source and the destination of their processing.
- Secondary storage is also much slower than main memory.

![[/Untitled 10.png|Untitled 10.png]]

As a general rule, there is a trade-off between size and speed, with smaller and faster memory closer to the CPU.

  

  

# I/O Structure

After setting up buffers, pointers, and counters for the I/O device, the device controller transfers an entire block of data directly to or from the device and main memory, with no intervention by the CPU. Only one interrupt is generated per block, to tell the device driver that the operation has completed, rather than the one interrupt per byte generated for low-speed devices. While the device controller is performing these operations, teh CPU is available to accomplishe other work.

  

  

# Computer-System Architecture

1. Single-Processor Systems

- The core is the components that executes instructions and registers for storing data locally. The one main CPU with its core is capable of executing a general-purpose instruction set, including instructions from processes. These systems have other special-purpose processors as well. They may come in the form of device-specific processors, such as disk, keyboard, and graphics controllers.
- These special-purpose processors run a limited instruction set and do not run processes. Sometimes, they are managed by the OS and sends them their next task and monitors their status. This arragement relieves the main CPU of the overhead of disk scheduling.
    - PCs contain a microprocessor in the keyboard to convert the keystrokes into codes to be sent to the CPU.
    - The OS cannot communicate wit hthese processors; they do their jobs autonomously.

  

1. Multiprocessor Systems

- The primary advantage of multiprocessor systems is increased throughput. When multiple processors cooperate on a task, a certain amount of overhead is incurred in keeping all the parts working correctly. This overhead, plus contention for shared resources, lowers the expected gain from additional processors.
- The most common multiprocessor systems use symmetric multiprocessing or SMP, in which each peer CPU processor performs all tasks, including OS functions and user processes. Each CPU processor has its own set of registers, as well as local or private cache. However, all processors share physical memory over the system bus.
    - The benefits of this model is that many processes can run simultaneously without causing performance to deteriorate significantly. However, since the CPUs are separate, one may be sittle idle.
- The definition of multiprocessor has evolved over time and now includes multicore systems, in which multiple computing cores reside on a single chip. Multicore systems can be more efficient than multiple chips with single cores because on-chip communication is faster than between-chip communication. Also, it uses less power.

![[/Untitled 1 4.png|Untitled 1 4.png]]

- Aside from architectural considerations, a multicore processor with N cores appears ot the OS as N standard CPUs.
- Adding additional CPUs to a multiprocessor system will increase computing power, but the concept does scale very well as contention for the system bus becomes a bottleneck and performance begins to degrade.
    - An alternative approach is to provide each CPU with its own local memory that is accessed via a small and fast local bus. The CPUs are connected by a shared system interconnect, so that all CPUs share ony physical address space. This is non-uniform memory access of NUMA.

  

# Clustered Systems

- Another type of multiprocessor system is a clustered system which gathers together multiple CPUs.
- Clustered systems differ from the multiprocessor systems in that they are composed of two or more individual systems or nodes joined together; each node is typically a multicore system. Such systems are considered loosely coupled.
- The generally accepted definition is that clustered computers share storage and are closely linked via a LAN.
- Clustering is usually used to provide high-availability service—that is, service that will continue even if one or more systems in the cluster fail.
- Generally, we obtain high availability by adding a level of redundancy in the system. High availability provides increased reliability. The ability to continue providing service proportional to the level of surviving hardware is called graceful degradation. Some systems go beyond graceful degradation and are called fault tolerant, because they can suffer a failure of any single component and still continue operation.
- Clustering can be structured asymme