  

![[/image 10.png|image 10.png]]

  

# Caching

- In general, a cache is a small, fast storage device that acts as a staging area for the data objects stored in a larger, slower device. The process of using a cache is known as caching.
- The central idea of a memory hierarchy is that for each k, the faster and smaller storage device at level k serves as a cache for the larger and slower storage device at level k+1. In other words, each level in the hierarchy caches data objects from the next lower level.

![[/image 1 7.png|image 1 7.png]]

- data are always copied back and forth between level k and level k+1 in block-size transfer units. while block size is fixed for any particular pair of adjacent levels, other pairs of levels can have different block sizes.
- in general, devices lower in the hierarchy have longer access times, and thus tend to use larger block sizes in order to amortize these longer access times.

  

### Cache Hits

- when a program needs a particular object d from level k+1, it first looks for d in one of the blocks currently stored at level k. if d happens to be cached at level k, then we have a cache hit. the program reads d directly from level k.

  

### Cache _Miss_

- if the data object d is not cached at level k then the cache at level k fetches the block containing d from the cache at level k+1, possibly overwriting an existing block if the level k cache is already full. once the block is fetched and stored at level k, it is then extracted to be used.
- the process of overwriting is known as replacing or evicting the block. it is a victim block. the decision about which block to replace is governed by the cache’s replacement policy.

  

### Kinds of Cache Misses

- if the cache at level k is empty, then any access of any data object will miss. an empty cache is a cold cache and they are called compulsory misses or cold misses. cold misses are essential because they are often transient events that might not occur in steady state, after the cache has been warmed up by repeated memory accesses.
- whenever there is a miss, the cache at level k must implement some placement policy that determines where to place the block it has retrieved from level k+1. hardware caches typically implement a simpler placement policy that restricts a particular block at level k+1 to a small subset of the blocks at level k.

  

## Cache Management

- at each level, some form of logic must manage the cache. something has to partition the cache storage into blocks, transfer blocks between different levels, decide hits and misses, and deal with them. the logic can be hardware, software or a combination.
- the compiler manages the register file, the highest level of the cache hierarchy.
- caches at level L1, L2, L3 are managed entirely by hardware logic built into the caches
- DRAM main memory serves for data blocks stored on disk, and is managed by a combination of OS software and address translation hardware on the CPU.
- In most cases, caches operate automatically and do not require any specific or explicit actions from the program.

![[/image 2 5.png|image 2 5.png]]

  

> [!important] Memory hierarchy based on caching works because slower storage is cheaper than faster storage and because programs tend to exhibit locality:
> 
>   
> Exploiting locality. because of temporal locality, the same data objects are likely to be reused multiple times. once a data object has been copied into the cache on the first miss, we can expect a number of subsequent hits on that object. since the cache is faster than the storage at the next lower leve, these subsequent hits can be served much faster than the original miss.  
> 
>   
> Exploiting spatial locality. blocks usually contain multiple data objects. because of spatial locality, we can expect that the cost of copying a block after a miss will be amortized by subsequent references to other objects within that block.  

  

  

### Anatomy or a Real Cache Hierarchy

- caches can hold instructions as well as data.
- a cache that holds instructions only is called an i-cache.
- a cache that holds program data only is called a d-cache.
- a cache that holds both instructions and data is known as a unified cache
- modern processors include separate i-caches and d-caches. with two-separate caches, the processor can read an instruction word and a data word at the same time.

  

  

# Writing Cache-Friendly Code

  

1. Make the common case go fast.
    1. focus on the inner loops of the core functions and ignore the rest
2. Minimize the number of cache misses in each inner loop