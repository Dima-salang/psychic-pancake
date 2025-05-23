Well-written programs tend to exhibit good locality. That is, they tend to reference data items that are near other recently referenced data items or that were recently referenced themselves. This is the principle of locality.

  

Locality has two distinct forms

- temporal locality
    - programs with good temporal locality optimizes that memory locations to be referenced once is more likely to be referenced multiple times in the future
- spatial locality
    - if a memory location is referenced once, then the program is likely to reference a nearby memory location in the near future.

  

> [!important] Programmers should always seek to exploit the principle of locality because programs with good locality run faster than programs with poor locality.

  

All levels of computer systems exploit locality. At the hardware level, main memory accesses are optimized by cache memories that hold blocks of the most recently referenced instructions and data items.

  

At the OS level, locality allows the system to use the main memory as a cache of the most recently referenced chunks of the virtual address space.

  

Web browsers and software can also exploit locality.

  

  

Programs that repeatedly reference the same variables enjoy good temporal locality

For programs with stride-k reference patterns, the smaller the stride, the better the spatial locality. Programs with stride-1 reference patterns have good spatial locality. Programs that hop around memory with large strides have poor spatial locality

loops have good temporal and spatial locality with respect to instruction fetches. the smaller the loop body and the greater the number of loop iterations, the better the locality.