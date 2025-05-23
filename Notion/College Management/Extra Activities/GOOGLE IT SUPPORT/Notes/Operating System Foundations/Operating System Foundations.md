---
Created: 2023-09-01T15:56
Type: Lecture
Reviewed: false
---
# Operating System

- the whole package that manages our computer’s resources and lets us interact with the UI.
- It is made up of two parts: the Kernel Space and the User Space

![[/Untitled 20.png|Untitled 20.png]]

  

## File Systems

- the kernel is also responsible for handling file storage and file systems

### Components

  

- File System
    - when using a new storage device, it needs to be erased and configured with a file system for the OS to r/w
    - different file systems have purposes and can affect storage capacity, speed, and resilience to file corruption.
        - Windows: NTFS
        - Linux: EXT4
    - Compatibility issues can arise when trying to move files between file systems, use the recommended file system for your OS.
- File Data
    - Data is written to storage devices in the form of data blocks
    - Data block may not be stored as a single continuous piece; they can be broken down into pieces and written to various locations on the disk.
    - Block storage enhances data retrieval speed and optimizes storage utilization.
- File Metadata
    - provides essential details about files and helps in file management and organization

  

## Process Management

- A process refers to a running program, such as a web browser or text editor, while a program is an application that can be executed.
    - multiple processes of the same program can run simultaneously each representing an instance of the program.
- Running programs require RAM and CPU resources but these are finite and efficient management is essential.
    - The kernel doesn’t allocate all resources to a single process but rather handles multiple processes simultaneously.
    - When a program needs o run, a corresponding process is created
    - Each process requires hardware resources and CPU time for execution.
- Multiple processes coexist, and the CPU executes them sequentially through a mechanism known as time slicing.
    - A time slice is an extremely short interval during which a process gets CPU time for execution.
    - The CPU executes processes very rapidly like milliseconds so that it appears to uses that everything runs simultaneously.
- Slow computer performance with maxed-out CPU resources can result from:
    - A single process consuming excessive time slices
    - An excessive number of processes competing for CPU time.
- Manual intervention may be necessary. The kernel is responsible for creating, scheduling, and terminating processes. And proper termination of processes is crucial to reclaim resources and allocate them to other processes.

![[/Untitled 1 9.png|Untitled 1 9.png]]

## Memory Management

- Virtual memory is a concept that combines hard drive space and RAM to provide more memory for processes than physically available
- When a process is executed, its data is divided into smaller units called pages.
    - These pages are stored in virtual memory or in the swap space.
- To execute a process, its pages must be transferred from virtual memory to physical RAM.
- Virtual Memory allows for efficient memory management, particularly for large applications.
    - Storing the entire program in RAM would be wasteful, as not all features are used simultaneously. You can notice that when clicking on a menu on an application that you haven’t used might slow down the page, this is because it is loading the pages necessary for that feature into RAM.
- The allocated space on the hard drive used for storing virtual memory is known as swap space.

![[/Untitled 2 6.png|Untitled 2 6.png]]

## I/O Management

- The kernel handles the loading of drivers necessary to recognize and communicate with various hardware components. It also manages the transfer of data to and from I/O devices.
- I/O devices often need to communicate with each other. The kernel facilitates intercommunication and determines the most efficient method of data transfer.
- Slow system performance is a common issue in IT support. Potential causes include insufficient RAM, CPU limitations, or excessive I/O operations that could introduce bottlenecks like if there is too much output than input, it might block other I/O operations.
- When you’re troubleshooting or solving a problem with a slow machine, it’s usually some sort of hardware resource deficiency.

  

## User Space

- is where human interactions take place

Two Methods of Interaction

- CLI
    - relies on text commands and responses
    - Often preferred by power users and essential in Linux environments
- GUI
    
    - provides a visual way to interact with the computer
    
      
    

## Logs

- are files that record system events on a computer
- serve as a system’s diary
- are essential for troubleshooting and diagnosing issues

  

## Boot Process

![[/Untitled 3 6.png|Untitled 3 6.png]]

  

Other booting methods:

  

Internal Method

- A drive can be partitioned to boot into different operating systems

  

External Tools:

- USB drives
- Optical media
- Solid state drive
- Network boot using Pre Boot Environment or PXE
- Internet-based booting

  

  

## Selecting an OS

- decision factors include existing organizational requirements, software compatibility, and hardware specifications.
- Consider the software needed, whether it’s specific to an OS or cross-platform
- Ensure compatibility with hardware and CPU architecture

  

## Virtual Machines

- are just a copy of a real machine
- use physical resources
- are easier to maintain and provision especially in IT settings.