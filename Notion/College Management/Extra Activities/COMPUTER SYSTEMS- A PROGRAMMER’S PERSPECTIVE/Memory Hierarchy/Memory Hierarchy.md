In practice, a memory system is a hierarchy of storage devices with different capacities, costs, and access times.

- CPU registers hold the most frequently used data.
- Small, fast cache memories nearby the CPU act as staging areas for a subset of the data and instructions stored in the relatively slow main memory.
- The main memory stages data on large, slow disks, which in turn often serve as staging areas for data stored on the disks or tapes of other machines connected by networks.

  

> [!important] If you understand how the system moves data up and down the memory hierarchy, then you can write your application programs so that their data items are stored higher in the hierarchy, where the CPU can access them more quickly.

- This idea is centered around the concept of locality.
    - programs with good locality tend to access the same set of data items over and over again, or they tend to access sets of nearby data items.
    - programs with good locality tend to access data items from the top of the hierarchy more often, and thus run faster.

  

## Storage Technologies

  

### Random Access Memory or RAM

- Static
    
    - Static RAM or SRAM is faster and significantly more expensive than DRAM
    - used for cache memories both on and off the CPU chip.
    - SRAM stores each bit in a bistable memory cell. Each cell is implemented with a six-transistor circuit. This circuit has the property that it can stay indefinitely in either of two different voltage configurations, or states. Any other state will be unstable—starting from there, the circuit will quickly move toward one of the stable states. It is like a pendulum.
    
    ![[/image 3.png|image 3.png]]
    
- Dynamic
    - is used for the main memory plus the frame buffer of a graphics system.
    - stores each bit as charge on a capacitor. This capacitor is very small. Each cell is consists of a capacitor and a single access transistor.
    - Unlike SRAM, a DRAM memory cell is very sensitive to any disturbance. when the capacitor voltage is disturbed, it will never recover. exposure to light rays will cause the capacitor voltages to change.

  

### Conventional DRAMs

The cells (bits) in a DRAM chip are partitioned into d supercells, each consisting  
of w DRAM cells. A d × w DRAM stores a total of dw bits of information. The  
supercells are organized as a rectangular array with r rows and c columns, where  
rc = d. Each supercell has an address of the form (i, j ), where i denotes the row  
and j denotes the column.  

![[/image 1 2.png|image 1 2.png]]

- each DRAM chip is connected to some circuitry known as the memory controller, that can transfer w bits at a time to and from each DRAM chip. To read the contents of supercell (i, j), the memory controller sends the row address i to the DRAM , followed by the column address j. The DRAM responds by sending the contents of supercell (i, j) back to the controller. The row address i is called row access strobe or RAS request The column address j is called column address strobe or CAS request.

![[/image 2 2.png|image 2 2.png]]

  

### Nonvolatile Memory

- DRAMs and SRAMs are volatile in the sense that they lose their information if the supply voltage is turned off. Nonvolatile memories, on the other hand, retain their information even when they are powered off.
- There are a variety of nonvolatile memories or ROMs, even though some types of ROMs can be written to as well as read. ROMs are distinguished by the number of times they can be reprogrammed or written to and by the mechanism for reprogramming them.

  

  

# Accessing Main Memory

- Data flows back and forth between the processor and the DRAM main memory over shared electrical conduits called buses.
- Each transfer of data between the CPU and memory is accomplished with a series of steps called a bus transaction.
    - A read transaction transfers data from the main memory to the CPU.
    - A write transaction transfers data from the CPU to the main memory.
- A bus is a collection of parallel wires that carry address, data, and control signals.
- Also, more than two devices can share the same bus. The control wires carry signals that synchronize the transaction and identify what kind of transaction is currently being performed.

Figure 6.6 shows the configuration of an example computer system. The main  
components are the CPU chip, a chipset that we will call an I/O bridge (which  
includes the memory controller), and the DRAM memory modules that make up  
main memory. These components are connected by a pair of buses: a system bus  
that connects the CPU to the I/O bridge, and a memory bus that connects the I/O bridge to the main memory. The I/O bridge translates the electrical signals of the  
system bus into the electrical signals of the memory bus. As we will see, the I/O  
bridge also connects the system bus and memory bus to an I/O bus that is shared  
by I/O devices such as disks and graphics cards. For now, though, we will focus on  
the memory bus.  

![[/image 3 2.png|image 3 2.png]]

  

Consider what happens when the CPU performs a load operation such as

  
movq A,%rax  

  
where the contents of address A are loaded into register %rax. Circuitry on the  
CPU chip called the bus interface initiates a read transaction on the bus. The  
read transaction consists of three steps. First, the CPU places the address A  
on the system bus. The I/O bridge passes the signal along to the memory bus  
(Figure 6.7(a)). Next, the main memory senses the address signal on the memory  
bus, reads the address from the memory bus, fetches the data from the DRAM,  
and writes the data to the memory bus. The I/O bridge translates the memory bus  
signal into a system bus signal and passes it along to the system bus (Figure 6.7(b)).  
Finally, the CPU senses the data on the system bus, reads the data from the bus,  
and copies the data to register %rax  

![[image 4.png]]

  

The same can be said for a write transaction. When you want to write data to A from the register %rax, then the CPU places the address of A on the memory bus to be read by the main memory. The main memory reads it and waits for the data word. The CPU places the data word y on the bus. Finally, the main memory reads data word y from the bus and stores it at address A.

![[image 5.png]]

  

  

[[Locality]]

[[College Management/Extra Activities/COMPUTER SYSTEMS- A PROGRAMMER’S PERSPECTIVE/Memory Hierarchy/Memory Hierarchy/Memory Hierarchy|Memory Hierarchy]]

[[Caching Schemes]]

[[Replacement Policies]]

[[Write Policies]]

[[College Management/Extra Activities/COMPUTER SYSTEMS- A PROGRAMMER’S PERSPECTIVE/Memory Hierarchy/Untitled|Untitled]]