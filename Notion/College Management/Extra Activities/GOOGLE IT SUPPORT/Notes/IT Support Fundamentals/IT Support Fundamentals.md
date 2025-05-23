---
Created: 2023-09-01T15:56
Type: Lecture
Materials:
  - "[[Chapter_1.pdf]]"
Reviewed: true
---
# What is IT?

- the use of digital technology, like computers and the Internet, to store and process data into useful information.

  

# History of Computing

> [!important] A
> 
> **computer** is a device that stores and processes data by performing calculations.

  

## Early Computing Devices

  

### Abacus

- the abacus, dating back to 500 BC, was one fhe earliest known computing tools.
- Blaise Pascal’s mechanical calculator automated basic arithmetic operations

### Jacquard Loom

- Joseph Jacquard’s programmable loom used punch cards to create fabric patterns
- these punch cards served as instructions for the loom.

### Charles Babbage

- developed the Difference Engine, an advanced mechanical calculator.
- it could perform complex mathematical operation and it laid out the foundation for the modern computer.
- Babbage followed this up with the Analytical Engine, inspired by punched cards, allowed predefined calculations.
- Ada Lovelace recognized its potential and developed the first algorithm for the engine and marked the birth of computer programming.

  

## The Path to Modern Computers

- when the war broke out, countries wanted to gain the power and edge over their neighbors.
    - During the war, computers were used for covert operations that involves cryptography.
    - Governents’ interest in computing research during the war led to significant progress.

  

### After WW2

- Companies like IBM and Hewlett Packard expanded computing technologies
- 20th-century computing advancements were fueled by government interests and scientific research

### Transition from Vacuum Tubes to Transistors

- early computers relied on vacuum tubes, which were bulky and unreliable.
- Transistors replaced vacuum tubes, making computers smaller and more efficient

  

![[/Untitled 21.png|Untitled 21.png]]

![[/Untitled 1 10.png|Untitled 1 10.png]]

### Impact of Compilers

- Admiral Grace Hopper invented the first compiler, enabling human-readable code to be translated into machine code.

### The Rise of Personal Computers

- PCs became more affordable and accessible, thanks to companies like Xerox.
- Steve Wozniak and Steve Jobs founded Apple Computer, making computers available to the average consumer.

![[/Untitled 2 7.png|Untitled 2 7.png]]

Apple II

- IBM introduced its PC, which led to a partnership with Microsoft and the development of MS DOS.

### Video Games

- the popularity of video games, starting with Pong
- Entertainment computers, like the Atari Video Computer System, brought gaming into homes.

### Open Source OS

- Richard Stallman’s GNU project aimed to create a free, open-source Unix-like OS.
- Linux, created by Linus Torvalds, evolved from GNU and became a significant player in the OS market
- Open source software is widely used today.

  

![[/Untitled 3 7.png|Untitled 3 7.png]]

Richard Stallman

![[/Untitled 4 5.png|Untitled 4 5.png]]

Linus Torvalds

![[/Untitled 5 5.png|Untitled 5 5.png]]

![[/Untitled 6 5.png|Untitled 6 5.png]]

GNU Project

  

  

### Mobile Computing and Smartphones

- PDAs introduced portable computing capabilities
- Nokia’s PDA with mobile phone functionality paved the way for smartphones

  

  

  

  

# Digital Logic

## Computer Language

### Binary System

- the language of the computer. also known as **base-2 numeral system.**
- we group binary into 8 bits (binary digits).
    
    - this is called a byte
    - each byte can store one character, and we can have 256 possible values ($2^8$)
    
      
    

## Character Encoding

- Assigns binary values to characters

### ASCII

- represents the English alphabet, digits, and punctuation in binary.
- has 127 characters.

  

### UTF-8

- based on the Unicode Standard
- offers a character encoding larger than ASCII and is based on a variable amount of bytes.

## Binary

- 0 and 1
- our computer can do binary calculations using electricity, transistors and logic gates
    - Logic gates allow transistors to do more complex tasks depending on logical conditions.
    - Logic gates can be linked so that the output of one gate serves as the input for other gates
    - Circuits are complex electrical systems built by linking logic gates together.

### Common Logic Gates

  

- NOT Gate
    
    - inverts the binary value. So if the input is 1, the output is 0.
    
    ![[/Untitled 7 4.png|Untitled 7 4.png]]
    
      
    
      
    
- AND Gate
    
    - outputs 1 only when both the inputs are 1. Otherwise, the output is 0.
    
    ![[/Untitled 8 3.png|Untitled 8 3.png]]
    
      
    
- OR Gate
    
    - Outputs 0 if both inputs are 0. Otherwise, the output is 1.
    
    ![[/Untitled 9 3.png|Untitled 9 3.png]]
    
- XOR Gate
    
    - Outputs 1 when only one of the inputs are 1. Otherwise, the output is 0.
    
    ![[/Untitled 10 2.png|Untitled 10 2.png]]
    
      
    
- NAND Gate
    
    - Outputs 0 only when both the inputs are 1. Otherwise, the output signal will be 1.
    
    ![[/Untitled 11 2.png|Untitled 11 2.png]]
    
      
    
- XNOR Gate

![[/Untitled 12 2.png|Untitled 12 2.png]]

  

  

  

# Computer Architecture Layer

## Abstraction

- to take a relatively complex system and simplify it for our use
- hides complexity by providing a common interface.
    - error messages are an abstraction.

  

A computer is divided into four main parts:

- Hardware
    - physical components of the system
- Software
    - How we as humans interact with computers
- Operating System
    - allows hardware to communicate with the system
- End Users
    - Interacts with the system
    - The most important part of IT is the human element.

  

  

# The Modern Computer

Desktops, Laptops, All-in-One Computers

- typical components include mice, keyboards, speakers, webcams, microphones, monitors,

### Ports

- connection points that we can connect devices to that extend the functionality of our computer.

  

### CPU

- the brain of the computer
- does all the calculations and data processing

  

  

## Programs, CPU, and Memory

- computers communicate in binary but humans use languages
    - to bridge the gap, computers rely on translation mechanisms

### Programs and Instructions

- Instructions that tell the computer what to do.
- Programs are stored on durable media like hard drives.
- The CPU processes these instructions, acting as the chef.

  

  

### RAM

- is the computer’s short-term memory.
- it stores instructions and data for the CPU to access quickly
- RAM allows the CPU to work efficiently by holding instructions for immediate use.

  

### Translating Instructions

- Instructions are broken down into binary code.
- Binary is sent one line at a time.

  

### Data Transmission

- the External Data Bus or EDB is a set of wires that transmit binary data within the computer
- Transistors help transmit voltage, allowing data to move through the EDB.

  

### Registers

- Registers are components within the CPU where data is temporarily stored
- Registers hold data for processing.
- Data is transferred between registers to execute instructions.

  

### Memory Controller Chip or MCC

- the MCC acts as a bridge between the CPU and RAM.
- It facilitates the retrieval of data from RAM by locating and sending it through the EDB.
- The address bus communicates the data’s location to the MCC.

  

### Cache Memory

- Cache refers to a small amount of recently used data that is stored either on hardware or in software.
    - The first time data is accessed, both the initial request for the data and the reply containing the data pass through multiple points in their journey.
    - Cache speeds up this process by holding a local copy of the most recently accessed data in temporary storage.
- Cache memory is smaller and faster than RAM.
- It stores frequently accessed data for quick retrieval.
- CPUs have multiple cache levels: L1, L2, and L3. L1 is the fastest.
    - CPU cache is normally stored inside each core of the CPU.
    - Level 3 Cache: is the largest and slowest of CPU cache but twice as fast as RAM. L3 is the first CPU cache location to store data after it is transferred from RAM. It is often shared by all of the cores in a single CPU.
    - Level 2 Cache: holds less data than L3 cache, but it has faster access speeds. It also holds a copy of the most recently access data that is not currently in use by the CPU. Each CPU core normally has its own L2 cache.
    - Level 1 Cache: L1 cache is the fastest and smallest of the three CPU cache levels. It holds the data currently in use by the CPU. Each CPU core usually has its own L1 cache.

  

### Synchronization

- CPUs have an internal clock and clock wire for synchronization
- Clock cycles are units of CPU operation.
- The CPU’s clock speed, measured in GHz, determines its maximum cycle rate.

  

  

  

  

![[/Untitled 13 2.png|Untitled 13 2.png]]

  

  

## CPUs

- is the central processing unit and serves as the computer’s brain.
- it uses an instruction set to translate and execute various functions on data.
- Complex computer programs are broken down into small, simple instructions in the instruction.

![[/Untitled 14 2.png|Untitled 14 2.png]]

### Instruction Sets

- serve as the language that CPUs understand and execute.
- They are a predetermined list of commands that a CPU can follow.
- Different CPU manufacturers may use unique instruction sets. However, most CPUs perform similar fundamental functions.

  

### CPU Manufacturers and Chipsets

- the choice of CPU depends on factors like intended use, performance requirements, and budget constraints.
- Each CPU manufacturer has its strengths and weaknesses.

  

### CPU Compatibility

- Compatibility between a CPU and a mobo is essential.
- CPUs come in various socket types,
    - LGA (Land-Grid Array) sockets have pins protruding form the mobo
    - PGA (Pin-Grid Array) sockets have pins on the processor itself.
- Manufacturers typically label the socket type on the product packaging, but check the mobo’s specification too.

  

### Heat Management

- CPUs generate heat during operation due to the electrical currents passing through their transistors.
- Heat sinks are commonly used to absord and disspate heat.
- Adequate cooling ensures that the CPU operates within its recommended temperature range and maintains consistent performance.

  

### CPU Architecture

- A 32-bit CPU efficiently handles 32 bits of data, while a 64-bit CPU can manage 64 bits at once.
- 64-bit architectures have become more prevalent due to their ability to handle large amounts of RAM and improved processing power.

  

## RAM

- serves as a computer’s short-term memory.
- volatile

### Functions

- stores data that is actively in use by programs and the OS.
- It acts as a workspace for the CPU, allowing it to process data efficiently.
- RAM is instrumental in multitasking since it enables the concurrent execution of multiple programs.

  

### Capacity

- When a computer specifies its RAM size, it indicates the amount of data it can hold at one time.
- More RAM allows for running a larger number of programs simultaneously without slowdowns.
- Activities like typing a document also utilize RAM to temporarily store the text data.

  

### Volatile Nature

- RAM data is transient and is lost when the computer loses power.
- the loss of unsaved work, is a result of RAM clearing its data.

  

### Types of RAM

- The commonly used RAM type in computers is DRAM or Dynamic RAM
- DRAM stores binary data in microscopic capacitors as charged or discharged states.
- DRAM chips are mounted on various types of memory sticks, with DIMM or Dual Inline Memory Module being a modern standard.

  

### Synchronous DRAM or SDRAM

- SDRAM is synchronized with the system’s clock speed, enhancing data processing speed.

  

### Double Data Rate SDRAM or DDR SDRAM

- often referred to as DDR, is the current standard for RAM in modern systems
- - DDR RAM is faster, more power-efficient, and has higher capacities.

  

### Compatibility

- RAM sticks must be compatible with the motherboard, particularly concerning the number of pins and the RAM slots.

  

## Motherboard

- serves as the foundation for a computer, enabling various parts to communicate, routing power, and providing expandability through expansion slots.

## Key Characteristics:

### Chipset

- is a set of integrated circuits on the motherboard that governs how different components communicate
- It typically consists of two main chips: the Northbridge and the Southbridge
    - Northbridge facilitates high-speed communication between the CPU, RAM, and GPU. In modern CPUs, the Northbridge functionality is often integrated into the CPU itself, eliminating the need for a separate Northbridge chipset.
    - The Southbridge manages I/O controllers, including hard drives, USB devices, and other peripherals.

  

### Peripheral Connectivity

- Motherboards provide connectors and ports for various external devices.

  

### Expansion Slots

- Expansion slots on motherboards provide the means to enhance a computer’s functionality by adding expansion cards.
- The current standard for expansion slots is PCIe or PCI Express.

  

  

### Form Factor

- Mobos come in different sizes which determine they physical dimensions and the number of components they can accommodate.
- the most common form factor is ATX or Advanced Technology Extended. Another is MicroATX
- the choice of form factor affects the available space for components and expansion slots, making it essential to select the right size for your needs.

  

### Impact on System Building

- it influences the type of RAM modules, CPU sockets, expansion slots, and overall expandability
- Deciding on the form factor also affects the physical size and potential use cases of the computer.

## Physical Storage Devices

### Hard Disk Drives or HDDs

- HDDs use a spinning platter and a mechanical arm to read and write data.
- Speed is determined by the platter’s rotation speed, measured in RPM or Revolutions Per Minute.
- Higher RPM results in faster data access
- HDDs have moving parts, making them more susceptible to physical damage.

### Solid State Drives or SSDs

- have no moving parts; data is stored on microchips.
- faster and more durable than HDDs
- Slimmer form factor compared to HDDs

  

### Pros and Cons

- HDDs are more cost-effective but prone to physical damage
- SSDs are less risky for data loss but generally more expensive.
- Hybrid drives combine SSD and HDD elements, offering SSD performance for critical tasks and HDD capacity for less important data storage.

  

### Drive Interfaces

### ATA Interfaces

- Among the most common interfaces for hard drives
- Serial ATA or SATA is a popular variant of ATA
- SATA drives use one cable for data transfers
- SATA drives are hot-swappable, allowing connection without turning off the computer.

![[/Untitled 15 2.png|Untitled 15 2.png]]

SATA Cable

![[/Untitled 16 2.png|Untitled 16 2.png]]

PATA Cable

  

### NVMe (Non-Volatile Memory Express)

- Designed to handle the high-speed demands of SSDs
- Instead of using a cable, NVMe drives are added as expansion slots

![[/Untitled 17 2.png|Untitled 17 2.png]]

  

## Power Supplies or PSUs

- Computers require a PSU to convert electricity from the wall into a usable form.
- Two types of electricity: DC and AC
- Computers use low-voltage DC power, necessitating the conversion of AC power from the wall.
    
    ### Components
    
    - Most PSUs include a fan for cooling
    - Voltage information is listed underneath or on the side of the PSU
    - Cables are provided to connect the PSU to the motherboard and a power outlet
    
    ### Importance of Proper Voltage
    
    - Incorrect voltage usage can damage electronic devices
    - Analogizing electricity to water pressure: Voltage represents pressure.
    - Higher voltage can lead to more electricity flow, potentially damaging devices.
        - If you plug in a device that needs 220-240 VAC but the wall socket delivers 110-120 VAC, there not enough power.
        - If you plug in a device that needs 110-120 VAC but the wall socket delivers 220-240 VAC, there is too much power and might fry the device.
    - It’s essential to match the device’s voltage requirements with the power source:
        - 110-120 VAC in North,Central and parts of South America
        - 220-240 VAC else in the world
    
    ### Understanding Amperage (Current)
    
    - Amperage, measured in amps, refers the amount of electricity pulled by a device
    - Voltage pushes electricity, while amperage pulls it.
    - Devices with higher amperage can charge faster
    
    ### Wattage
    
    - Wattage is the product of voltage and amperage, indicating the power a device needs
    - Inadequate wattage in a PSU can lead to computer issues

  

## Mobile Devices

- they are known for their mobility, portability, and battery-powered operation.
- Mobile devices can be:
    - General-purpose computing devices
        - Tablets, smartphones
    - Task-specific computing devices
        - Fitness watches, e-readers
- Mobile devices are highly integrated, making it impossible or challenging to disassemble them.
    - Smaller devices tend to have more integrated components.
    - Some mobile devices use a System on Chip or SoC, which combines the CPU, RAM, and sometimes storage on a single chip.
- Mobile devices can also be peripherals.
- Mobile devices may employ standard or proprietary ports and connectors.
- Mobile devices run specialized operating systems and applications optimized for their performance and limited power.
- Privacy is paramount when handling mobile devices especially in BYOD scenarios.

  

## Batteries

- Mobile devices rely on rechargeable batteries to provide power when not connected to a power outlet.
- Rechargeable batteries have a limited lifespan measured in change cycles (one full charge and discharge).
- As batteries age, they may take longer to charge and hold less charge compared to when new.
- Mismatching chargers and devices can damage the battery, the device itself, and the charger.
- Extreme temperatures can damage rechargeable batteries
    
    - Charging or discharging batteries outside their safe operating temperature range is not advisable.
    - Damaged batteries can swell, rupture, or even pose a fire hazard.
    
      
    
    ### Charging Process
    
    - An external power source is required
    - A charging circuit similar to a PSU manages the power transfer to the batter

  

  

## Peripherals

- Peripherals are external devices connected to a computer to add functionality

### USB

- USB has evolved over time
- Speed is measured in Mb/s, not MB.
- One byte equals eight bits so 8 Mb/s is required to transfer 1MB of data in a second.
- USB ports are backward compatible, meaning older devices work with newer ports.
- USB 2.0 ports are typically black (480 Mbps or 35/40 MB/s), USB 3.0 (5 Gbps or 300 MB/s) are blue, and USB 3.1 ports are teal (10 Gbps or 1.2 GB/s).

![[/Untitled 18 2.png|Untitled 18 2.png]]

- There are various types of USB connectors, including Type-A, Type-B, Mini-USB, Micro-USB, and the more recent Type-C which is becoming a universal standard for both data transfer and power delivery.

  

### Micro USB, USB-C and Lightning Port

- they are smaller connectors that carry more power than older USB connectors and have faster data transfer speeds.
    
    - Micro USB is a small USB port found on many non-Apple devices
    - USB-C is the newest reversible connector. USB-C cables replace traditional USB connectors since they can carry significantly more power and transfer data at 20 Gbps.
    - USB4 uses Thunderbolt 3 protocol and USB-C cables to transfer data at speeds of 40 Gbps and provide power as well.
    - Lightning Port is a connector exclusive to Apple that is similar to USB-C.
    
    ![[/Untitled 19 2.png|Untitled 19 2.png]]
    

  

### Display Peripherals

- DVI
    - outputs video but may not support audio
- HDMI
    - standard for both video and audio output
    - Widely used in television and multimedia connections
- DisplayPort
    - another standard for audio and video output.

  

## BIOS

- Basic devices like mice and keyboards don’t provide instructions that the CPU understands. They need drivers.
- The BIOS software is software that initializes the hardware components during computer startup. It helps get the OS up and running.
    - BIOS is stored in a non-volatile ROM chip on the mobo.
- UEFI (Unified Extensible Firmware Interface) is a modern replacement for traditional BIOS, with better hardware compatibility
- POST (Power-On-Self-Test)
    
    - is a hardware test run by the BIOS or UEFI during computer startup. It checks if all hardware components are functioning correctly before the OS loads.
    - POST runs before fully initializing any hardware or loading essential drivers. Main steps.
        - CPU Check
        - BIOS ROM Check
        - Memory Check
        - Device Check
        - Hardware Configuration Check
    - If there’s an issue, it typically produces a series of beep codes to identify the problem. Different manufacturers may have unique beep codes.
    
    ![[/Untitled 20 2.png|Untitled 20 2.png]]
    
- BIOS Settings and CMOS Chip
    - The CMOS chip stores basic computer data, including date, time, and startup preferences
    - Users can access and modify BIOS settings through the BIOS or UEFI settings menu.

  

  

  

## INSTALLING PROCESSORS

- Prevent ESD by grounding yourself or use an anti-static wristband connected to a non-painted metal surface.
- Keep computer components in anti-static bags until installation.

### Motherboard Installation

- choose the appropriate form factor
- Match the holes on the motherboard to those on the desktop case
- Install standoffs

  

### CPU Installation

- Carefully remove the CPU from its anti-static bag.
- Ensure the CPU matches the mobo socket type
- Align the CPU correctly with the socket, ensuring markers match
- Secure the CPU in the socket with a bit of force.

### Heat Sink and Thermal Paste

- Apply a small amount of thermal paste evenly on the CPU
- Attach the heat sink to the CPU, aligning the screws with the socket
- Tighten the screws in an opposite sides pattern to ensure secure attachment
- Connect the heat sink fan to the mobo via the molex connector. This controls the fan speed.