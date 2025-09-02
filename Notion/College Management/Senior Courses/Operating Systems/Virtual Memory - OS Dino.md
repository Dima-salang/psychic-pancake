Perfect — now we are entering one of the **most important topics in Operating Systems**: **Virtual Memory (VM)**.
This is the foundation of how modern systems manage programs much larger than RAM, enable process isolation, efficient sharing, and more. I’ll walk you through this as if I’m giving a **graduate-level lecture**, covering both **theory** and **real-world implementations**.

---

# **Chapter 10: Virtual Memory**

---

## **10.1 Background**

### **Motivation**

From Chapter 9, you already know the basic **memory-management strategies**:

* **Contiguous allocation** (simple, but causes fragmentation).
* **Paging** (eliminates external fragmentation, allows flexible allocation).
* **Segmentation** (logical division, but still complex to manage).

All these assumed one core requirement:
👉 **A process must be entirely in physical memory to execute.**

At first glance, this feels reasonable — the CPU can only fetch instructions and data if they’re in RAM. But this creates **two major problems**:

1. **Program size limitation**: The maximum program size ≤ available physical memory.

   * If your RAM is 4 GB, you can’t run a program that needs 8 GB.

2. **Inefficiency**: Even when the program fits, **not all of it is needed at once**.

   * Error-handling code may never run.
   * Arrays may be declared huge but mostly unused.
   * Optional modules may never be touched.

Thus, requiring *all of a program* to be in RAM wastes memory and limits usability.

---

### **The Idea of Virtual Memory**

Virtual Memory (VM) solves this by **decoupling logical memory from physical memory**:

* **Logical / Virtual memory**: what the programmer sees.

  * Large, contiguous, starts at address 0, grows with stack and heap.

* **Physical memory**: actual RAM frames.

  * Smaller, fragmented, not contiguous.

👉 The **Memory Management Unit (MMU)** maps **virtual pages** to **physical frames**.
👉 Only the **active portions** of a process need to reside in RAM.
👉 The rest lives in **backing store** (disk/SSD).

Result:

* Programs can be **larger than physical memory**.
* **Many more processes** can coexist in memory at once.
* Programmer is freed from worrying about memory limits.

---

### **Benefits of Virtual Memory**

1. **Programs larger than RAM** → no artificial size constraint.
2. **Higher degree of multiprogramming** → better CPU utilization and throughput.
3. **Less I/O overhead** → only bring in pages as needed.
4. **Simplifies programming**: programmers see a clean, huge, linear address space.
5. **Supports shared memory and libraries**:

   * Libraries (like libc) can be **mapped into multiple processes** (read-only shared).
   * Shared memory for IPC (inter-process communication) is easily implemented.
   * Pages can even be shared **at process creation** (e.g., `fork()`).

---

### **Virtual Address Space**

Each process has a **Virtual Address Space (VAS)**:

* Logical view: looks contiguous, starts at 0.
* Actually mapped to non-contiguous frames in RAM.

Example layout (Figure 10.2):

```
0 ---------------------
|   Text (code)       |
|   Data (global vars)|
|   Heap (grows up)   |
|       ...           |
|   (unused / hole)   |   <-- Sparse address space
|       ...           |
|   Stack (grows down)|
----------------------- max
```

* **Heap grows upward** (dynamic memory via `malloc`, `new`).
* **Stack grows downward** (function calls, local variables).
* The **hole** in between is part of the virtual address space but only consumes physical frames if needed → **sparse address spaces**.

This is powerful because it:

* Supports **dynamic growth** (stack/heap).
* Supports **dynamic linking** (loading shared libraries into holes at runtime).
* Avoids wasting RAM for unused space.

---

### **Virtual Memory and Sharing**

With VM, sharing is easy (Figure 10.3):

* Libraries can be **mapped read-only** into multiple processes.
* Shared memory regions (for IPC) are mapped into multiple processes’ virtual address spaces, but point to the **same physical frames**.
* Process creation (`fork()`) is sped up — child and parent initially share pages until they are written to (**copy-on-write**).

---

### **Key Takeaway**

Virtual memory is not just about "running big programs on small RAM."
It is about **abstraction, flexibility, and efficiency**:

* Abstraction → programmers see one large, contiguous memory space.
* Flexibility → processes only need active pages in RAM.
* Efficiency → supports sharing, avoids waste, and allows oversubscription.

---

✅ Up to this point, we’ve covered **what virtual memory is and why it matters**.
The next natural step is **how it is implemented** — which leads us into **Demand Paging (10.2)**, where the system only loads pages when they are needed.

---

Would you like me to continue straight into **Demand Paging (10.2)**, where we’ll cover page faults, lazy loading, and the mechanics of execution with only partial memory?



Excellent — now we are getting into **Demand Paging (Section 10.2)**, which is the *practical backbone* of how modern operating systems make virtual memory work efficiently. Let’s carefully go through this step by step, highlighting **definitions, mechanisms, hardware/OS support, and performance implications**.

---

# **10.2 Demand Paging**

---

## **Motivation**

When a program is launched, one naive approach is to load **the entire executable image into physical memory**.

* Problem: much of that code/data may never actually be used (error handlers, rarely used features, oversized arrays, etc.).
* This wastes RAM and slows startup.

👉 **Solution: Demand Paging**

* Only load a page *when it is actually accessed*.
* Pages that are never touched are never brought into RAM.
* This is a direct consequence of virtual memory: processes can run even if only part of their virtual address space is resident in RAM.

---

## **10.2.1 Basic Concepts**

### **Definition**

**Demand Paging** = A paging technique in which pages are loaded into memory **only when they are referenced (on demand)**, instead of preloading the entire process.

* Backing store: secondary storage (HDD, SSD, or NVM) holds non-resident pages.
* In RAM: only active pages (working set) are loaded.

This improves efficiency but requires **hardware + OS cooperation**.

---

### **Valid–Invalid Bit Mechanism**

Each **page-table entry (PTE)** has a **valid–invalid (V/I) bit**:

* **Valid (V = 1):** Page is in RAM and legal to access.
* **Invalid (V = 0):** Either:

  1. The page is **not part of the process’s logical address space** (illegal access).
  2. The page is valid logically, but it’s currently only on disk (not resident).

👉 When a process references an **invalid** entry → **Page Fault**.

---

### **Page Fault Handling (Trap to OS)**

Steps when a page fault occurs (Figure 10.5):

1. **Hardware detects fault**: CPU tries to access a virtual address → PTE invalid → hardware raises a trap.
2. **OS checks validity**:

   * If the reference is illegal → process is terminated (**segmentation fault**).
   * If valid but page is on disk → continue.
3. **Find a free frame** (from the free-frame list).
4. **Schedule I/O**: read the page from backing store into RAM.
5. **Update page table**: mark entry as valid, record frame number.
6. **Restart instruction**: The faulting instruction is re-executed, now succeeding since the page is in RAM.

👉 Key requirement: **instruction restartability**.
The CPU must resume execution as if the fault never happened, except now the page is available. This requires saving **registers, program counter, condition codes, etc.**

---

### **Pure Demand Paging**

In the extreme, a process may start with **no pages in RAM**.

* The very first instruction fetch causes a page fault.
* As execution proceeds, pages are faulted in one by one.
* Eventually, the working set of the program is loaded, and execution stabilizes.

This is called **pure demand paging**.

---

### **Instruction Restarting Issues**

Most instructions can be retried safely after a page fault (simply refetch instruction and operands). But:

* **Simple case:** `ADD A, B → C`

  * If page fault on C, reload page, restart instruction, redo fetch & add.

* **Complex case:** IBM System/360 `MVC` (move up to 256 bytes).

  * If partially completed before fault, restarting is tricky.
  * Solutions:

    1. Hardware pre-accesses all pages before starting.
    2. Use **temporary registers** to store modified values, restoring them if a fault occurs → ensures memory is unchanged until instruction fully succeeds.

👉 This shows **demand paging requires architecture support**, not just OS tricks.

---

## **10.2.2 Free-Frame List**

To satisfy page faults, the OS maintains a **free-frame list**:

* A pool of unused physical frames.
* When a fault occurs → take one frame from the list, load the new page into it.

### **Zero-Fill-on-Demand**

Before allocation, frames are cleared (filled with zeros).

* Prevents accidental **information leakage** (security issue).
* Ensures consistent memory state.

At **system startup**, all memory is placed on the free-frame list. As frames are allocated, the list shrinks. If it empties → replacement algorithms must be used (covered later in Section 10.4).

---

## **10.2.3 Performance of Demand Paging**

### **Effective Access Time (EAT)**

Demand paging introduces delays. Let:

* **ma** = memory access time (e.g., 200 ns).
* **p** = probability of a page fault.
* **page fault service time** = disk access + OS handling (\~8 ms for HDD).

Formula:

$$
EAT = (1 - p) \times ma + p \times (page\ fault\ service\ time)
$$

Example:

* **ma = 200 ns**
* **page fault service time ≈ 8 ms = 8,000,000 ns**

So:

$$
EAT = 200 + 7,999,800 \times p
$$

👉 If **p = 0.001** (1 in 1000 accesses faults):

$$
EAT \approx 8.2 \mu s \quad \Rightarrow \quad 40 \times slowdown!
$$

👉 To keep slowdown < 10%:

$$
p < 0.0000025 \quad \text{(fewer than 1 page fault in ~400,000 accesses)}  
$$

⚠️ **Conclusion**: Page faults must be **extremely rare** for good performance.

---

### **Swap Space Considerations**

* **Swap space**: area on disk reserved for paging (faster than filesystem I/O because large contiguous allocation is used).
* **Strategies**:

  1. Copy entire program image to swap space at startup, then page from swap.

     * Faster paging later, but startup overhead.
  2. Page from **file system initially**, then write to swap as needed (used by Linux, Windows).
  3. Read-only pages (e.g., program text/code) can be discarded instead of written to swap → reload from file if needed.

---

### **Mobile OSes (iOS, Android, etc.)**

* Generally, **do not use swapping** (storage wear, energy cost).
* Use demand paging from file system only.
* **Read-only pages** (code) can be dropped and reloaded.
* **Anonymous pages** (stack, heap) stay in RAM or are reclaimed only when app is terminated.
* Some systems (e.g., iOS) use **compressed memory** instead of swapping (discussed in Section 10.7).

---

# **Summary of Demand Paging**

* **Demand Paging** = load pages only when needed → saves memory, improves multiprogramming.
* **Hardware support**: valid-invalid bits, page tables, ability to restart instructions.
* **OS responsibility**: handle page faults, maintain free-frame list, manage swap space.
* **Performance challenge**: Page faults are **orders of magnitude slower** than memory access → must keep fault rate *extremely low*.
* **Practical implementations**:

  * Use swap space for fast paging.
  * Discard code pages (reload from file instead of swapping).
  * Mobile OSes avoid swapping entirely.

---

✅ We now have a **deep, detailed understanding of demand paging**: how it works, why it’s efficient, and what makes it dangerous if mismanaged.

Would you like me to move on to **Page Replacement (10.3 & 10.4)**, where we study what happens when the free-frame list runs out and the OS must choose a victim page?


Perfect — this is one of the most *critical* sections in virtual memory: **Page Replacement** (Section 10.4). This is where demand paging meets real-world constraints. Let me walk you through it as I would in a graduate OS lecture, covering **theory, mechanisms, algorithms, and practical considerations**.

---

# **10.4 Page Replacement**

### **The Problem**

Earlier in demand paging, we assumed each page faults *only once* when first referenced.
But in reality:

* Processes don’t use *all* their pages all the time.
* The degree of **multiprogramming** can be increased (more processes sharing limited RAM).
* If processes suddenly expand their active working set, we may need *more frames than exist*.
* Buffers, I/O, and other kernel data structures also consume RAM, making things tighter.

Thus, **over-allocation** happens: all frames are in use, and a page fault occurs. The OS must somehow bring in the needed page. But where to put it?

👉 This leads us to **page replacement**.

---

## **10.4.1 Basic Page Replacement**

When a page fault occurs and no free frames are available, the OS must:

1. **Locate the desired page** on secondary storage (backing store).
2. **Find a free frame**:

   * If available, use it.
   * If not, run a **page-replacement algorithm** to pick a **victim frame**.
   * If the victim page is dirty (modified), write it back to disk.
   * Update page table and frame tables.
3. **Read the desired page** into the frame.
4. **Restart the process** at the faulting instruction.

This extends the page-fault service routine: sometimes we need *two I/O transfers* (page-out + page-in), which **doubles service time**.

---

### **The Modify Bit (Dirty Bit) Optimization**

Each frame/page has a **modify (dirty) bit**, set by hardware when the page is written.

* If dirty = 1 → must write page back before replacing.
* If dirty = 0 → no need to write; page on disk is already up-to-date.
* Read-only pages (code, shared libraries) are *always non-dirty* → can be discarded without writing.

This reduces replacement overhead significantly.

---

### **Why Replacement is Essential**

* Without replacement: all pages of a process must reside in RAM.
* With replacement: only a subset (its **working set**) is needed at any moment.
* This allows **logical address space > physical memory**, the essence of **virtual memory**.

---

### **Two Core Problems in Demand Paging**

1. **Frame allocation**: how many frames does each process get?
2. **Page replacement**: which victim page should be chosen on a fault?

The rest of this chapter focuses on replacement policies.

---

---

## **10.4.2 FIFO Page Replacement**

### **Algorithm**

* Associate each page with the time it was loaded.
* Maintain a **queue** of pages in memory.
* On replacement: remove the oldest (front of queue).

This is simple, but sometimes performs poorly.

---

### **Example**

Reference string:

$$
7, 0, 1, 2, 0, 3, 0, 4, 2, 3, 0, 3, 2, 1, 2, 0, 1, 7, 0, 1
$$

with 3 frames.

* First 3 accesses (7, 0, 1) → all faults (frames fill).
* Next access (2) → replace 7 (FIFO).
* Next access (0) → already in memory, no fault.
* Next access (3) → replace 0 (it was oldest).
* … and so on.

👉 Total: **15 page faults** (shown in Figure 10.12 of the book).

---

### **Observations**

* Easy to implement (simple queue).
* May discard *frequently used pages* just because they were loaded long ago.
* Leads to unnecessary page faults.

---

### **Belady’s Anomaly**

Surprisingly, for FIFO:

* Increasing the number of frames may **increase** the number of page faults.
* Example reference string:

  $$
  1, 2, 3, 4, 1, 2, 5, 1, 2, 3, 4, 5
  $$

  * With 3 frames → 9 faults.
  * With 4 frames → 10 faults (!).

This is **Belady’s Anomaly** (Figure 10.13).

* Counterintuitive: more memory doesn’t always mean fewer faults.
* FIFO is not a **stack algorithm** (stack algorithms never suffer from this anomaly, e.g., LRU).

---

# **Summary So Far**

* **Page replacement** is essential when no free frames exist.
* **Modify (dirty) bit** optimization reduces overhead.
* **FIFO replacement**: simple but naive, suffers from **Belady’s anomaly**.

---

👉 Next, the text will cover **Optimal (OPT) replacement** and **LRU (Least Recently Used)**, which solve FIFO’s shortcomings and avoid Belady’s anomaly.

---

Would you like me to continue into **Optimal and LRU page replacement (10.4.3 & 10.4.4)**, so you see how real systems improve on FIFO?


Excellent — you’ve brought us to the heart of **page replacement policies**, where the theory of virtual memory meets practical OS design. Let me walk you through **Optimal, LRU, LRU-Approximation, and Counting-Based replacement**, keeping very close to the material while adding depth where useful.

---

# **10.4.3 Optimal Page Replacement (OPT / MIN)**

### **Definition**

* The **optimal page replacement algorithm** is the one that achieves the **minimum possible page-fault rate** for a given reference string and number of frames.
* Rule:

  > **Replace the page that will not be used for the longest period of time in the future.**

### **Properties**

* **Never suffers from Belady’s anomaly.**
* Provides a **benchmark**: no algorithm can perform better.
* Like **Shortest Job First (SJF) scheduling**, it requires **future knowledge**, which makes it **impractical** in real systems.

### **Example**

Reference string:

```
7, 0, 1, 2, 0, 3, 0, 4, 2, 3, 0, 3, 2, 1, 2, 0, 1, 7, 0, 1
```

With **3 frames**:

* First 3 references (7,0,1) → faults, fill frames.
* Next reference (2) → must replace. Which one?

  * Page **7** is replaced, since it will not be used again until reference 18.
  * Pages **0** and **1** are needed sooner.

Continuing this logic, **OPT produces only 9 faults** (vs FIFO’s 15).

### **Key Takeaway**

* **Not implementable** (no future knowledge).
* **Used as a gold standard**: to measure how close practical algorithms (like LRU) are to optimal.

---

# **10.4.4 Least Recently Used (LRU) Replacement**

### **Definition**

* Approximation of OPT.

* Rule:

  > Replace the page that has not been used for the **longest period of time in the past**.

* Intuition: *recent past ≈ near future*.

* Thus, **LRU ≈ OPT**, but uses *past history* instead of future prediction.

---

### **Example (Same Reference String)**

With **3 frames**:

* First 5 faults are identical to OPT.
* At reference to page **4**, LRU evicts page **2**, since it was least recently used (even though 2 will be needed very soon).
* Result: **12 faults**.

👉 Still much better than FIFO’s 15, but worse than OPT’s 9.

---

### **Implementation Approaches**

LRU requires tracking "last used time" for each page, which can be costly. Two standard implementations:

1. **Counters (Logical Clock)**

   * Each PTE has a "time-of-use" field.
   * Every memory reference → update this field with a global counter.
   * On replacement → scan for the **smallest time value**.
   * Issues:

     * Requires updating memory metadata on *every access*.
     * Requires scanning all PTEs → costly.
     * Counter overflow handling needed.

2. **Stack (Doubly Linked List)**

   * Maintain pages in a stack (MRU at top, LRU at bottom).
   * Every reference → move page to top.
   * Replacement → evict bottom page.
   * Efficient for finding victim, but moving elements requires pointer manipulation.
   * Works well in software or microcode implementations.

---

### **Properties**

* **Does not suffer Belady’s anomaly.**
* Belongs to **stack algorithms**:

  * For any reference string, the set of pages in memory with *n* frames is always a subset of the set with *n+1* frames.
  * Thus, more frames can’t increase faults.

### **Problem**

* True LRU requires **substantial hardware support**.
* Software-only LRU would require trapping on *every memory access*, which is impractical (slows memory by 10×).
* Real systems often use **approximations** (see next section).

---

# **10.4.5 LRU-Approximation Algorithms**

Since exact LRU is too expensive, OSes use **hardware-supported approximations**. Most rely on the **reference bit**:

* **Reference bit**:

  * Set to `1` by hardware whenever a page is accessed (read/write).
  * Cleared periodically by the OS.
  * Indicates whether the page has been recently used.

---

## **(1) Additional-Reference-Bits Algorithm**

* Keep an **8-bit shift register** per page.
* At fixed intervals (e.g., every 100 ms via timer interrupt):

  * Shift the register right by 1.
  * Insert current reference bit into high-order position.
  * Clear the reference bit.
* Result: register encodes recent usage history.

  * `00000000` → unused for 8 intervals.
  * `11111111` → used every interval.
* On replacement → choose page with smallest register value.
* This provides a **historical approximation of LRU**.

---

## **(2) Second-Chance Algorithm (Clock Algorithm)**

* Based on FIFO queue, but checks **reference bit**.
* Procedure:

  * Inspect oldest page (front of queue, or "hand of clock").
  * If reference bit = `0` → replace it.
  * If reference bit = `1` → clear it, move page to back of queue, continue scanning.
* Result: pages get a "second chance" before eviction.
* If all bits = `1`, behaves like FIFO.

---

## **(3) Enhanced Second-Chance Algorithm**

* Uses both **reference bit** and **modify (dirty) bit**.
* Four categories (ordered best → worst):

  1. `(0,0)` → not used recently, clean → **best candidate**.
  2. `(0,1)` → not used recently, dirty → must write back first.
  3. `(1,0)` → used recently, clean → keep if possible.
  4. `(1,1)` → used recently, dirty → keep if possible.
* Algorithm scans circular queue, picking the lowest nonempty class.
* Reduces I/O by preferring clean pages.

---

# **10.4.6 Counting-Based Algorithms**

Instead of time-based history, we can count **how often** each page is referenced.

### **LFU (Least Frequently Used)**

* Replace page with **lowest reference count**.
* Assumes frequently used pages are important.
* Problem:

  * Pages heavily used early may remain forever (stale counts).
  * Fix: use **exponentially decaying counts** (shift counts periodically).

### **MFU (Most Frequently Used)**

* Replace page with **highest reference count**.
* Rationale: a page with a low count was likely just brought in and not yet used.
* Rarely used in practice.

### **Reality Check**

* Both LFU and MFU are **expensive to implement** and do not approximate OPT well.
* Rarely used in real systems.

---

# ✅ **Summary**

* **Optimal (OPT/MIN):** Theoretical minimum page faults; requires future knowledge → impractical.
* **LRU:** Practical approximation of OPT, based on past history. Avoids Belady’s anomaly.
* **LRU Implementation:** Counters or stacks, but costly; needs hardware.
* **LRU Approximations:**

  * Additional Reference Bits → shift registers.
  * Second-Chance (Clock) → FIFO + reference bits.
  * Enhanced Second-Chance → considers dirty pages.
* **Counting-Based:** LFU, MFU → conceptually interesting, but rarely useful.

---

👉 Next section (10.5) transitions into **allocation of frames** — where the OS decides *how many frames each process should get*, which is just as crucial as the replacement policy.

Would you like me to continue with **frame allocation (10.5)**, or pause here so you can digest page replacement fully?


Perfect — now we’re in the **heart of virtual memory management: frame allocation.** This is where the OS takes the fixed pool of physical page frames and decides **how many each process should get, when they can take from each other, and how hardware constraints shape those choices.**

Let’s carefully work through **10.5 Allocation of Frames** in detail, keeping both the theoretical rules and real-world practices in focus.

---

# **10.5 Allocation of Frames**

We now move beyond *which* page to replace (page-replacement policies from §10.4) and look at a deeper issue:

👉 **How does the OS allocate the finite set of physical page frames among all running processes?**

This is crucial, because:

* Too few frames per process → excessive page faults (possibly thrashing).
* Too many frames reserved for one process → waste of memory and slowdown for others.

---

## **Basic Allocation Strategy**

Imagine a machine with **128 total frames**.

* OS needs **35 frames** for itself (kernel, buffers, tables).
* Leaves **93 frames** for user processes.

Under **pure demand paging**:

* The **free-frame list** initially has all 93.
* First 93 page faults of a process → allocated from this free pool.
* After exhaustion, page replacement kicks in.
* When process exits, its frames return to the free list.

🔑 Key idea: **processes draw frames from the free pool**, but when the pool runs out, a replacement policy determines which frames to recycle.

---

## **10.5.1 Minimum Number of Frames**

Allocation is **constrained** by:

1. **Maximum frames per process** = limited by physical memory.
2. **Minimum frames per process** = dictated by **computer architecture** and **instruction set**.

### Why a Minimum?

* Page-fault rate skyrockets if frames are too few.
* Even worse: some instructions **cannot complete without a minimum number of frames.**
* Example:

  * If each instruction references 1 memory address → need at least **2 frames**: one for the instruction, one for the operand.
  * If one-level indirect addressing is allowed → may need **3 frames** (instruction, operand pointer, actual data).
  * On architectures where instructions or operands can straddle frames → need **up to 6 frames**.

📌 **Architecture-defined minimum is a hard lower bound.**
E.g., Intel x86/x86-64 avoids direct memory-to-memory moves → fewer frames required than some CISC architectures.

---

## **10.5.2 Allocation Algorithms**

Now that we know we need **at least N frames**, the OS must decide how to distribute available memory. Two main strategies:

### **1. Equal Allocation**

* Each of `n` processes gets **m/n** frames (where `m` = total free frames).
* Example: 93 frames, 5 processes → each gets 18, with 3 leftover as buffer.
* Simple, but **ignores process needs.**

### **2. Proportional Allocation**

* Frames given in proportion to process size.
* Let `si` = size of process *i* in pages.
* Let `S = Σ si` (sum of all process sizes).
* Process *i* gets:

  $$
  a_i = \frac{s_i}{S} \times m
  $$
* Example:

  * Process A: 10 pages.
  * Process B: 127 pages.
  * Free frames = 62.
  * A gets ≈ 4 frames, B gets ≈ 57.

This avoids wasting memory on small processes.

### **3. Priority-Based Allocation**

* Variation of proportional allocation, but based on **priority weights** instead of size.
* High-priority process may be given **more frames** than its size alone would justify.
* Useful when system responsiveness matters (e.g., interactive vs batch jobs).

---

## **10.5.3 Global vs Local Allocation**

Once frames are assigned, **replacement policy scope** matters:

### **Global Replacement**

* A process may select *any* frame in the system (even from another process).
* Pros:

  * Higher system throughput.
  * Flexible use of free memory.
* Cons:

  * A process’s performance depends on the behavior of others.
  * Execution time of same program may vary drastically run-to-run.

### **Local Replacement**

* A process may only evict pages from *its own allocation*.
* Pros:

  * Performance is stable and predictable (depends only on own reference string).
* Cons:

  * System may waste memory: some processes’ frames sit unused while others thrash.

📌 **Most real OSes use global replacement** (better throughput), often with **priority tweaks**.

---

## **Major vs Minor Page Faults**

Page faults are not all equal:

* **Major page fault (hard fault):**

  * Page not in memory at all.
  * Requires I/O from disk (backing store).
  * Very expensive.

* **Minor page fault (soft fault):**

  * Page not mapped in this process’s page table, but is **already in memory**.
  * Example:

    * Shared library already loaded by another process.
    * Page reclaimed but still resident in free list.
  * Just needs to update page table → **much faster**.

👉 On Linux:

* `ps -eo min_flt,maj_flt,cmd` shows per-process counts.
* Typically: **minor faults are far more common than major faults** (thanks to shared libraries).

---

## **Threshold-Based Reclaiming (Global Policy Strategy)**

Instead of waiting for free frames to hit **zero**, many OSes maintain:

* **Minimum threshold:** when free memory drops below → kernel starts reclaiming (page reaper daemon).
* **Maximum threshold:** reclaiming stops when free memory rises above.

See **Figure 10.18**:

* At point (a): free memory drops below min → reaper starts.
* At point (b): free memory above max → reaper stops.
* Cycles between (c) and (d).

### **Aggressive Reclaiming**

* If memory pressure is extreme, OS may:

  * Switch to simpler replacement policy (FIFO).
  * In Linux: invoke the **OOM Killer**.

    * Terminates a process with high “OOM score” (based on % memory use).
    * Frees memory immediately.

---

## **10.5.4 Non-Uniform Memory Access (NUMA)**

So far we’ve assumed **all memory is equally accessible**. On **NUMA systems**:

* Multiple CPUs, each with local memory.
* Accessing local memory = faster.
* Accessing remote memory = slower (higher latency).

### Implications for Frame Allocation:

* OS must allocate memory **close to the CPU where the process runs.**
* Scheduler must try to keep a process on the same CPU as before.

### OS Implementations:

* **Linux:**

  * Maintains **per-NUMA-node free frame lists**.
  * Uses **scheduling domains** → tries to keep process + memory local.
* **Solaris:**

  * Defines **lgroups** (locality groups) of CPUs + memory.
  * Process scheduled and memory allocated within one lgroup whenever possible.

🔑 Goal: **maximize locality, minimize cross-node traffic, improve cache hit rates.**

---

# ✅ **Summary**

* **Basic Allocation:** Frames drawn from free list; when exhausted → replacement policy kicks in.
* **Minimum Frames:** Architecture-defined (instruction set may demand 2–6 frames at least).
* **Allocation Algorithms:**

  * Equal share (simple).
  * Proportional (based on process size).
  * Priority-based (size + priority).
* **Global vs Local Replacement:**

  * Global = flexible, high throughput, but unpredictable.
  * Local = predictable, but may waste memory.
* **Page Fault Types:**

  * Major (disk I/O) vs Minor (in-memory, just remap).
* **Threshold Reclaiming:** Keep free frames between min/max thresholds. Use reapers or OOM killer under pressure.
* **NUMA Systems:** Allocate frames “close” to CPU where process runs to reduce latency.

---

👉 Next, in **§10.6 Thrashing**, we’ll explore what happens when the OS allocates *too few frames overall* and page faults dominate execution (processes spend more time paging than computing).

Would you like me to move into **Thrashing** next, or pause here so we can work examples of **allocation algorithms** (equal vs proportional vs priority) with real numbers?


Perfect — now we’re deep into **§10.6 Thrashing**, which is one of the most important and practical problems in virtual memory management. Let’s carefully unpack it step by step, keeping the structure, definitions, and technical details exactly as the material presents, but with deeper explanation so you can fully understand both the theory and its real-world implications.

---

# **10.6 Thrashing**

---

## **Definition**

* **Thrashing** occurs when a process does not have “enough” frames (below the **minimum needed for its working set**) and is therefore **spending more time servicing page faults than doing actual computation.**
* In other words: the CPU is idle, while the paging device (disk/SSD) is overloaded.

👉 A **process is thrashing** if the majority of its time is spent paging, not executing instructions.

---

## **10.6.1 Cause of Thrashing**

Let’s walk through the classic feedback loop that causes thrashing:

1. **OS Monitors CPU Utilization:**

   * Operating systems often use CPU utilization as a signal of system health.
   * If CPU utilization is **low**, the OS assumes the system is underloaded → it **increases multiprogramming** (admits more processes).

2. **Global Page Replacement Policy:**

   * With global replacement, processes can **steal frames from each other.**

3. **Process Needs More Frames:**

   * Suppose one process enters a new execution phase (e.g., calling a new function, allocating a new array).
   * It needs more frames than currently allocated.
   * It starts faulting → steals frames from others.

4. **Cascading Page Faults:**

   * Now those other processes also fault (since they lost pages they still need).
   * All processes start competing for frames, thrashing each other.

5. **Paging Device Saturation:**

   * Paging device queue fills.
   * Processes wait for disk I/O.
   * Ready queue empties.

6. **CPU Utilization Drops:**

   * CPU sits idle while processes wait for paging.
   * Scheduler sees “low CPU usage” and makes the problem worse by admitting more processes.

7. **Result: Thrashing.**

   * Page-fault rate skyrockets.
   * Effective memory access time increases dramatically.
   * System throughput plunges.

👉 This vicious cycle is shown in **Figure 10.20**:

* Initially, as multiprogramming increases, CPU utilization increases.
* Beyond a point, adding more processes causes thrashing → CPU utilization **collapses**.

---

## **10.6.2 Locality Model**

How do we avoid thrashing? We need to understand the **locality model**:

* **Locality:** A set of pages actively used together during some execution phase.
* Programs naturally exhibit **temporal and spatial locality**:

  * Example:

    * Calling a function = new locality (instructions + local variables + subset of globals).
    * Exiting the function = leaving that locality.
* Localities often overlap (e.g., global pages appear in many localities).

### Key Point:

If a process has enough frames to hold its **current locality**, it will fault only when switching to a new locality.
If it has fewer frames than its locality size → **it thrashes.**

---

## **10.6.3 Working-Set Model**

The **working-set model** is a formalization of the locality principle.

* Define a parameter **Δ (working-set window)** = number of most recent memory references to consider.
* The **working set (WS(t))** at time *t* is the set of pages referenced in the last **Δ references**.

### Properties:

* If a page is actively in use → it will appear in the working set.
* If it is no longer used → it drops out after Δ references.
* Working set is therefore an **approximation of the locality.**

Example (Figure 10.22):

* Δ = 10.
* At t₁: WS = {1,2,5,6,7}.
* At t₂: WS = {3,4}.

### Choosing Δ:

* Too small → misses the full locality.
* Too large → merges multiple localities.
* Δ → ∞ = all pages touched during process lifetime.

---

### **Working-Set Size (WSSᵢ):**

* For process *i*, **WSSᵢ = size of its current working set**.

* Total demand for frames =

  $$
  D = \sum WSS_i
  $$

* If **D > m** (total available frames):

  * Not enough frames to satisfy all working sets → **thrashing** occurs.

---

### **OS Policy with Working Sets**

* Allocate each process at least **WSSᵢ frames**.
* If extra frames exist, admit more processes.
* If **Σ WSSᵢ > m**:

  * Suspend a process.
  * Write its pages to backing store.
  * Reallocate freed frames to active processes.

👉 This keeps CPU utilization high and prevents thrashing.

---

### **Relationship to Page-Fault Rate**

* Entering a new locality → spike in page faults (peak).
* Once working set loaded → page faults drop (valley).
* Moving to next locality → new spike.

This explains the **oscillating pattern of page-fault rate** as programs progress.

---

## **10.6.4 Page-Fault Frequency (PFF)**

The **Page-Fault Frequency strategy** provides a simpler, direct way to prevent thrashing:

* Thrashing = **high page-fault rate**.
* Define **upper and lower bounds** on acceptable fault rate.

Policy:

* If fault rate > upper bound → allocate more frames.
* If fault rate < lower bound → reclaim frames.

If no free frames are available:

* Swap out a process to free frames.
* Redistribute to processes with high fault rates.

👉 This dynamically controls frame allocation by monitoring the **page-fault frequency curve** (Figure 10.23).

---

## **10.6.5 Current Practice**

In modern systems:

* **Main strategy = avoid thrashing altogether by provisioning enough physical memory.**
* From smartphones to servers, RAM is cheap → OS designers prefer to size memory so that **all working sets fit in RAM most of the time.**
* Swapping and thrashing are still possible under extreme load, but they are treated as **last resorts** (with OOM killer, memory compression, etc.).

---

# ✅ **Summary of Thrashing**

* **Thrashing:** Process spends more time paging than executing (page-fault storm).
* **Cause:** Too many processes + global replacement + insufficient frames.
* **Locality Model:** Programs work in localities. If frames ≥ locality size → low fault rate. If not → thrashing.
* **Working-Set Model:**

  * Defines active pages in last Δ references.
  * WSSᵢ = working set size of process i.
  * If Σ WSSᵢ > m → thrashing inevitable.
* **Page-Fault Frequency:**

  * Control thrashing by keeping page-fault rate within bounds.
  * Add frames if too high, remove if too low.
* **Current Practice:** Provision systems with enough memory to keep all working sets in memory concurrently. Thrashing is rare in well-provisioned systems.

---

👉 Now here’s my suggestion:
Would you like me to **work through a concrete example** (say, 3 processes with given reference strings, different Δ, and show how WSS, D, and thrashing detection works), or would you like me to move to the next section (**10.7 Memory Compression**) straight away?


Excellent — now we’re at **§10.8 Allocating Kernel Memory**, a subtle but critical section because kernel memory allocation is *not the same* as user-level memory allocation. The kernel must be far more careful due to its interaction with hardware, varying object sizes, and the need for efficiency. Let’s go through it systematically with the same depth as the material, but expanded with expert-level clarity and teaching commentary.

---

# **10.8 Allocating Kernel Memory**

When user processes request memory (e.g., via `malloc`), the kernel satisfies them by allocating pages from the **free page list**. These pages can be anywhere in physical memory and are typically managed by paging and page-replacement algorithms (§10.4).

But **kernel memory allocation is special**, and the OS often uses a *different pool* of free memory for the kernel. Why?

---

## **Why Kernel Memory Allocation is Special**

1. **Kernel allocates data structures of varying sizes**

   * Many kernel structures are **much smaller than a page** (e.g., process descriptors, inodes, semaphores).
   * If we always allocated whole pages (4 KB) for small structures, we would waste huge amounts of memory due to **internal fragmentation**.
   * Fragmentation is especially problematic because kernel memory is precious, and many systems **do not page out kernel memory** at all. Once allocated, it must stay resident.

2. **Some kernel memory must be contiguous in physical memory**

   * User processes can have noncontiguous pages because the MMU maps them into a contiguous virtual address space.
   * But some **hardware devices** (e.g., DMA engines, network cards, GPU buffers) interact **directly with physical memory**.
   * These devices may require large chunks of **physically contiguous pages**, which the standard user-level paging mechanism cannot guarantee.

👉 Therefore: **the kernel must manage its own allocation strategy**, separate from ordinary user processes. Two classic strategies are used:

* **Buddy system**
* **Slab allocation**

---

## **10.8.1 Buddy System**

### **Overview**

* The buddy system allocates memory from a **large fixed contiguous segment**.
* It uses a **power-of-2 allocator**:

  * Allocation units are always powers of 2 (e.g., 4 KB, 8 KB, 16 KB …).
  * If a request is not a power of 2, it is rounded **up** to the nearest power of 2.
  * Example: a 21 KB request is rounded up to **32 KB**.

### **How It Works**

1. Start with a large segment, e.g., 256 KB.
2. To satisfy a 21 KB request:

   * Split 256 KB → two 128 KB buddies (A<sub>L</sub>, A<sub>R</sub>).
   * Split 128 KB → two 64 KB buddies (B<sub>L</sub>, B<sub>R</sub>).
   * Split 64 KB → two 32 KB buddies (C<sub>L</sub>, C<sub>R</sub>).
   * Allocate C<sub>L</sub> (32 KB) to the request.

   (This is shown in **Figure 10.26**.)

### **Coalescing**

* When a block is freed, it can be recombined with its buddy if the buddy is also free.
* Example:

  * Free C<sub>L</sub> → coalesce with C<sub>R</sub> = 64 KB block (B<sub>L</sub>).
  * If B<sub>R</sub> is also free, coalesce → 128 KB block (A<sub>L</sub>).
  * Eventually, you can reconstruct the original 256 KB block.

This makes **freeing and merging memory efficient.**

### **Drawback: Fragmentation**

* The buddy system’s main weakness is **internal fragmentation**.
* Example:

  * Request = 33 KB → must allocate 64 KB block.
  * At least 31 KB wasted (≈ 48%).
* In fact, **worst-case internal fragmentation can be nearly 50%.**

👉 The buddy system is simple, fast, and supports coalescing, but is **wasteful**.

---

## **10.8.2 Slab Allocation**

The slab allocator was invented to eliminate the waste of the buddy system and to speed up kernel object allocation.

### **Basic Concepts**

* A **slab** = one or more contiguous physical pages.
* A **cache** = a collection of one or more slabs.
* Each cache stores only one type of **kernel object** (e.g., process descriptors, semaphores, file objects).
* Within each slab: memory is pre-divided into **equal-size chunks** the size of the object.
* Each chunk is an **object instance**, which can be **free** or **used**.

👉 Example:

* A 12 KB slab (3 contiguous pages) for 2 KB objects.
* Slab can hold **six 2 KB objects**.
* Initially, all objects marked **free**.
* When the kernel requests a new object, allocator simply marks one as **used**.

### **Linux Example**

* The process descriptor (`struct task_struct`) requires ≈ 1.7 KB.
* When Linux creates a new process, it allocates a `task_struct` from the **task\_struct cache**, not by allocating a new page.
* Objects come directly from preallocated slabs.

### **Slab States**

* **Full** – all objects in use.
* **Empty** – all objects free.
* **Partial** – mix of used and free.

Allocation policy:

1. Allocate from a **partial slab** if possible.
2. If none → allocate from an **empty slab**.
3. If no empty slabs exist → allocate a new slab from contiguous physical memory.

### **Advantages**

1. **No internal fragmentation**

   * Each cache corresponds to one object type, so memory is split exactly into object-sized chunks.
   * No rounding up to power-of-2.

2. **Fast allocation/deallocation**

   * Objects are pre-created and managed in caches.
   * Allocation = grab a free object → O(1).
   * Deallocation = mark object free and return to cache → O(1).
   * Very efficient for frequently created/destroyed objects (like processes, semaphores).

👉 This is why slab allocation became the preferred kernel allocator.

---

## **Kernel Allocators in Practice (Linux & Solaris)**

* **Solaris:** Slab allocation introduced in Solaris 2.4; later extended to handle certain **user-mode memory requests** as well.

* **Linux:**

  * Originally used the **buddy system** for kernel objects.
  * From Linux **2.2 onward** → adopted **slab allocator (SLAB)**.
  * Later, new allocators introduced:

    * **SLOB (Simple List of Blocks):**

      * For small-memory embedded systems.
      * Maintains **3 free lists**: small (<256B), medium (<1024B), large (< page).
      * Uses **first-fit allocation**.

    * **SLUB (Unqueued SLAB):**

      * Introduced in Linux **2.6.24** as default allocator.
      * Simplifies SLAB by removing per-CPU queues and storing metadata in page structures.
      * Reduces overhead → better scalability for **multi-core systems**.

---

# ✅ **Summary of Kernel Memory Allocation**

* Kernel allocation is separate from user allocation because:

  1. Kernel often needs small objects (avoid fragmentation).
  2. Kernel sometimes needs contiguous physical memory (for hardware).

* **Buddy System:**

  * Power-of-2 allocator.
  * Fast splitting/merging (coalescing).
  * Suffers from up to 50% internal fragmentation.

* **Slab Allocation:**

  * Organizes memory into **slabs** grouped in **caches**, each cache for one object type.
  * Allocation is O(1), no internal fragmentation.
  * Particularly good for frequently created kernel objects.

* **In practice:**

  * Solaris introduced slab allocation → widely adopted.
  * Linux now defaults to **SLUB allocator** (fast, scalable).
  * Embedded Linux may still use **SLOB** for small systems.

---

👉 Would you like me to go further by drawing a **side-by-side comparison table (Buddy vs Slab vs SLUB/SLOB)** so you have a crisp at-a-glance summary for exams/interviews, or do you prefer I continue straight into **§10.9 Other Considerations**?


Perfect — you’ve given me the **10.9 Other Considerations** section, which comes right after kernel memory allocation. Let’s go through this step by step, carefully and in detail, focusing on the **key definitions, reasoning, trade-offs, and practical aspects** you need to know.

---

# 🔹 10.9 Other Considerations in Paging Systems

When designing a **paging system**, the most important decisions are usually:

1. The **replacement algorithm** (like LRU, FIFO, Clock, etc.)
2. The **allocation policy** (how many frames each process gets, global vs. local replacement, etc.)

But, there are **other subtle issues** that significantly affect performance. These are the topics discussed in this section.

---

## 10.9.1 Prepaging

* **Problem:**

  * In pure **demand paging**, when a process starts execution, *every page reference causes a page fault initially* because nothing is in memory yet.
  * This leads to a **burst of page faults** (called the **cold start problem**) before the working set is established.

* **Solution: Prepaging**

  * Instead of waiting for faults, bring some (or all) of the process’s likely-needed pages into memory at once.
  * Example: If we know the working set of the process from before suspension, we can reload the *entire working set* when the process resumes.

* **Trade-off:**

  * Let’s say we prepage `s` pages, and only a fraction `α` are actually used.
  * Then:

    * We save `s × α` page faults.
    * But we waste memory + I/O for `s × (1 − α)` unnecessary pages.
  * If `α` is close to 0 → prepaging **hurts**.
  * If `α` is close to 1 → prepaging **helps**.

* **Practical use:**

  * Hard for executables (unpredictable access patterns).
  * Easier for files (often accessed sequentially).
  * Example: Linux provides **`readahead()` system call**, which prefetches file data into memory.

---

## 10.9.2 Page Size

* Page size is **fixed by hardware**, but when designing a machine/OS, page size choice is crucial.
* Always a **power of 2** (e.g., 4 KB, 8 KB, 2 MB, etc.).

### 🔹 Trade-offs in choosing page size

1. **Page Table Size**

   * Smaller pages → more pages → bigger page table.
   * Example: For 4 MB address space:

     * With 1 KB pages → 4096 pages.
     * With 8 KB pages → only 512 pages.
   * Since each process has its own page table, smaller pages = bigger overhead.
   * **Big pages reduce page table size.**

2. **Internal Fragmentation**

   * Each process rarely ends exactly at a page boundary.
   * On average, **half a page is wasted per process**.
   * Larger pages → more wasted memory.
   * **Small pages minimize fragmentation.**

3. **I/O Time**

   * Reading/writing a page from disk involves:

     * **Seek time** (\~5 ms)
     * **Rotational latency** (\~3 ms)
     * **Transfer time** (depends on page size).
   * Since seek+latency dominate, transfer time is negligible.
   * Larger pages → fewer I/Os needed.
   * **Large pages reduce I/O overhead.**

4. **Locality & I/O Amount**

   * Smaller pages → memory more accurately matches program’s locality.
   * Example: Process is 200 KB but only uses 100 KB:

     * With 200 KB pages → must load whole 200 KB.
     * With 1 KB pages → only load the 100 KB actually needed.
   * **Small pages improve locality & reduce wasted I/O.**

5. **Page Fault Frequency**

   * Very small pages → excessive page faults.
   * Very large pages → fewer faults, but more useless data loaded.

**Conclusion:**

* There’s no universal best size.
* Historically: 4 KB → very common.
* Modern trend: **larger page sizes** (even MBs), since memory and workloads are bigger.

---

## 10.9.3 TLB Reach

* Recall: **TLB hit ratio** = fraction of address translations resolved in the TLB.

* **TLB Reach** = `#entries × page size` → amount of memory directly accessible via TLB.

* Ideally, the **working set** of the process should fit in the TLB.

### How to increase TLB reach:

1. **More entries** → but expensive (TLB = associative memory).
2. **Bigger page size** → increases reach automatically.

   * E.g., 4 KB → 64 entries → reach = 256 KB.
   * 16 KB pages → same TLB → reach = 1 MB.
   * Downside: fragmentation.
3. **Multiple page sizes** (common in modern CPUs).

   * Linux: 4 KB (default), 2 MB (huge pages), even 1 GB (for large DBs).
   * ARMv8: allows **contiguous mapping** in TLB to extend reach.

**Trade-off:**

* Bigger pages = better TLB reach, but risk of **fragmentation**.

---

## 10.9.4 Inverted Page Tables

* **Problem:** Per-process page tables can be very large.

* **Solution:** Use an **inverted page table**:

  * One entry **per physical frame**, not per virtual page.
  * Each entry indexed by `<process-id, page-number>`.

* **Advantages:** Saves memory, since table size = #physical frames, not #virtual pages.

* **Disadvantages:**

  * Does not store *full* virtual address mapping.
  * Need an **external page table per process** to locate virtual pages on disk.
  * If page fault occurs, may require paging in the external table itself → can cause a **page fault inside a page fault handler**, requiring careful kernel handling.

---

## 10.9.5 Program Structure

* Demand paging is meant to be transparent, but program behavior affects performance.

**Example:**

* Array initialization:

  ```c
  for (j = 0; j < 128; j++)      // column-major order
      for (i = 0; i < 128; i++)
          data[i][j] = 0;
  ```

  * Accesses memory in **column-major order**, so every access jumps to a different page.
  * With 128 pages and not enough frames → **16,384 page faults!**

* If written as row-major:

  ```c
  for (i = 0; i < 128; i++)      // row-major order
      for (j = 0; j < 128; j++)
          data[i][j] = 0;
  ```

  * Accesses each page sequentially.
  * Only **128 page faults**.

**Moral:**

* **Locality of reference** matters.

* Good structures: **stacks, arrays accessed sequentially.**

* Bad structures: **hash tables, scattered references.**

* Compilers/loaders can also optimize paging by:

  * Keeping routines inside one page.
  * Grouping frequently called routines together.
  * Generating **reentrant (read-only) code** → clean pages never need to be written back.

---

## 10.9.6 I/O Interlock and Page Locking

* **Problem:** I/O devices (like disks, NICs) use **DMA (Direct Memory Access)** to transfer data directly to memory.
* Suppose:

  * A process requests I/O → buffer in memory is chosen.
  * That page gets swapped out before I/O finishes.
  * The I/O device writes to the *wrong page* now → **data corruption**.

### Two solutions:

1. **Copying:**

   * Always copy data between **user memory ↔ system (kernel) memory**.
   * I/O is only to kernel memory.
   * Safe, but expensive (extra copying).

2. **Page Locking (preferred):**

   * Add a **lock bit** per frame.
   * Locked pages cannot be replaced by the page replacement algorithm.
   * After I/O finishes, unlock the pages.

### Uses of Locking:

* Kernel code/data often locked (kernel must not page fault).
* Applications like **databases** may request pages to be pinned.
* Some OSes (like Solaris) allow “locking hints,” but may ignore them if memory is tight.

### Pitfalls:

* If a bug leaves lock bit permanently set → memory leaks (unusable frames).
* Also used in policy: e.g., prevent replacing a page that has *just been brought in* before it’s even used once.

---

# ✅ Summary of Key Takeaways

* **Prepaging:** reduces initial faults but may waste memory.
* **Page Size:** trade-off between table size, fragmentation, I/O efficiency, and fault rate.
* **TLB Reach:** want working set in TLB → increase via entries, larger pages, or multiple page sizes.
* **Inverted Page Tables:** save memory but require external tables for page faults.
* **Program Structure:** memory access patterns (row-major vs column-major) drastically affect page faults.
* **Page Locking:** prevents corruption during I/O, ensures kernel stability.

---

👉 Would you like me to also create a **comparison table** for page size trade-offs (small vs. large page size) so you can memorize the pros and cons quickly?

Excellent — you’ve asked me to take **Section 10.10 Operating-System Examples** (Linux, Windows, Solaris) and explain it to you in my capacity as both a senior systems engineer and a professor. I’ll stick closely to the material, but expand on it with the **technical detail, reasoning, and background theory** that makes it all click.

Let’s dive in carefully.

---

# 🔹 10.10 Operating-System Examples

This section shows how **real operating systems** (Linux, Windows, Solaris) implement virtual memory. While the theory we studied earlier (paging, replacement, working sets, etc.) applies to _all_ systems, each OS makes slightly different design choices depending on goals: performance, scalability, or reliability.

---

## 🟢 10.10.1 Linux

### 1. Memory Management Model

- Linux uses **demand paging**.
    
    - Pages are brought into memory **only when referenced**.
        
    - This reduces startup costs but means _initial references trigger page faults_.
        
- Pages are allocated from a **free-frame list** maintained by the kernel.
    

### 2. Replacement Policy

- Linux uses a **global replacement policy**, not per-process.
    
- It is an approximation of **LRU** (Least Recently Used) implemented with the **Clock algorithm** (Section 10.4.5.2).
    

### 3. Active vs. Inactive Lists

- Linux maintains two main lists of pages:
    
    - **Active list:** Pages currently considered "in use".
        
    - **Inactive list:** Pages not recently referenced, and thus candidates for reclamation.
        
- Each page has an **accessed (referenced) bit** (set by hardware on most CPUs).
    
    - When a page is first used → bit set → page goes to the **rear of active list**.
        
    - If page is referenced again → moves to the **rear of active list** (to represent recent use).
        
    - Periodically, the kernel resets accessed bits. Eventually, least-recently-used pages drift to the **front of active list**, and may migrate to inactive list.
        
    - If a page in the **inactive list** is accessed again → it goes back to the rear of the active list.
        

👉 This is basically a **two-tier approximation of LRU**:

- Active list = "hot" pages.
    
- Inactive list = "cold" pages (eligible for reclaim).
    

### 4. Balance & Page-Out Daemon

- Linux balances the sizes of active/inactive lists.
    
- If the active list grows too large → pages migrate to inactive.
    
- **kswapd (kernel swap daemon)** runs in the background:
    
    - Wakes up periodically.
        
    - Checks system’s **free memory level**.
        
    - If below a threshold → scans **inactive list** and reclaims pages (puts them back on free list).
        

✅ **Key takeaway:** Linux virtual memory = **two-list LRU approximation with demand paging, global replacement, and a page daemon (kswapd)**.

---

## 🔵 10.10.2 Windows (Windows 10)

Windows 10 supports both **32-bit and 64-bit architectures**, but memory management principles are the same across both.

### 1. Virtual Address Space

- **32-bit processes:** Default 2 GB (can be extended to 3 GB).
    
- **64-bit processes:** Vast — 128 TB of virtual space.
    
- Physical memory limits:
    
    - Windows 10 client: up to 24 TB.
        
    - Windows Server: up to 128 TB.
        

### 2. Features

Windows implements all the major VM features:

- **Demand paging.**
    
- **Copy-on-write.**
    
- **Shared libraries (DLLs).**
    
- **Paging to disk.**
    
- **Memory compression** (recent addition).
    

### 3. Paging with Clustering

- Windows handles **page faults with clustering**:
    
    - Instead of bringing in just the faulting page, it also prefetches neighboring pages (spatial locality).
        
    - For data pages: **cluster size = 3 pages** (faulting page + neighbors).
        
    - For other types: **cluster size = 7 pages**.
        
- This reduces future page faults by anticipating locality.
    

### 4. Working-Set Model

- Each process has a **working set** (set of resident pages).
    
- At creation, a process gets:
    
    - **Min:** 50 pages guaranteed.
        
    - **Max:** 345 pages.
        
- If memory is plentiful, processes may exceed their max.
    
- In memory pressure, processes may shrink _below their min_.
    
- This is **flexible working-set allocation** (not rigid).
    

### 5. Page Replacement

- Replacement algorithm: **Clock (LRU-approximation)**.
    
- Replacement uses both **local and global policies**:
    
    - **Local:** If a process exceeds its working-set maximum → it must replace its own pages (local policy).
        
    - **Global:** If system-wide free memory drops below a threshold → Windows uses **automatic working-set trimming**.
        

### 6. Automatic Working-Set Trimming

- If memory is low:
    
    - Virtual memory manager reclaims pages across processes.
        
    - Idle/larger processes lose pages first.
        
    - Continues trimming until free memory rises above threshold.
        
- Both **user processes and system processes** are trimmed.
    

✅ **Key takeaway:** Windows VM = **demand paging with clustering + working-set model + adaptive trimming**, mixing **local and global replacement**.

---

## 🟠 10.10.3 Solaris

Solaris takes a slightly different approach, with a strong emphasis on **continuous scanning**.

### 1. Page Allocation

- On page fault: kernel assigns a free page from free list.
    
- Therefore, Solaris must **always maintain sufficient free pages**.
    

### 2. lotsfree Threshold

- Parameter `lotsfree` ≈ 1/64 of physical memory.
    
- If free pages drop below `lotsfree` → **pageout process** begins.
    

### 3. Pageout Process (Two-Handed Clock)

- Solaris uses a **two-handed clock algorithm** (variation of second-chance).
    
    - **Front hand:** clears reference bits.
        
    - **Back hand:** checks bits later.
        
        - If still 0 → page reclaimed (and written to disk if dirty).
            
- Pages not reclaimed may "survive" if referenced again between hands.
    

### 4. Scan Rate

- The **rate of scanning** (pages per second) is adaptive:
    
    - **slowscan** = 100 pages/sec.
        
    - **fastscan** = half of physical pages/sec (max 8,192 pages/sec).
        
- As free memory drops, scan rate increases.
    

### 5. desfree and minfree

- **desfree:** Desired free-memory level.
    
    - If free memory < desfree → pageout runs **100 times/sec** (instead of 4).
        
- **minfree:** Absolute minimum threshold.
    
    - If memory < minfree → pageout invoked on _every page request_.
        

### 6. Swapping

- If pageout can’t maintain desfree for 30 sec → Solaris resorts to **swapping entire processes** (idle ones first).
    
- This frees all their pages at once.
    

### 7. Other Refinements

- **Shared library pages** are skipped during scanning (since many processes rely on them).
    
- **Priority paging**: distinguishes process pages from file-cache pages. Process pages are preserved preferentially.
    

✅ **Key takeaway:** Solaris VM = **two-handed clock + adaptive scan rate + desfree/minfree thresholds + swapping fallback**, with optimizations for libraries and file caches.

---

# 📊 Comparison: Linux vs. Windows vs. Solaris

|Feature|Linux|Windows 10|Solaris|
|---|---|---|---|
|**Replacement**|Global, two-list (active/inactive), Clock LRU approx.|Clock (LRU approx.), hybrid local+global|Two-handed clock (second chance variant)|
|**Prefetch / Clustering**|Demand paging only|Yes – clusters of 3 or 7 pages|Demand paging, no clustering|
|**Working-Set Model**|Implicit via lists|Explicit working-set min/max (50–345 pages, flexible)|Not explicit; relies on free-list thresholds|
|**Daemon / Background**|`kswapd` trims inactive list|Automatic working-set trimming when free mem low|`pageout` daemon, scans memory 4–100 times/sec|
|**Swapping**|Rare (modern Linux avoids it except extreme cases)|Avoided, trims first|If desfree unmet for 30 sec → swaps whole processes|
|**Optimizations**|Active/inactive list balance|Clustered I/O, adaptive working sets|Priority paging, skips shared libs|

---

✅ Now you see:

- **Linux**: Simple two-list global LRU approximation.
    
- **Windows**: Working-set based, clustered paging, trims adaptively.
    
- **Solaris**: Continuous scanning with thresholds, aggressive fallback to swapping.
    

---

Would you like me to now **explain this from an exam-prep perspective** (key points to memorize for each OS), or do you want me to go even deeper into **how these differences evolved historically and why each OS chose its strategy**?