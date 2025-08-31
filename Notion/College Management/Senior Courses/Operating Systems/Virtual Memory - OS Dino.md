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
