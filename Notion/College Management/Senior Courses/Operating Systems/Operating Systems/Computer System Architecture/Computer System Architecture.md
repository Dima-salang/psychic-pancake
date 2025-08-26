Perfect, let’s dig deep into **Computer-System Architecture (Section 1.3)** and break it down into a **structured lecture format** with **technical depth, diagrams in words, and examples** so you fully understand **single-processor systems, multiprocessor systems, NUMA, multicore, and clustered systems**.

---

# 🖥️ 1.3 Computer-System Architecture

An operating system (OS) sits on top of **computer hardware**. But the way the hardware itself is structured—the **system architecture**—heavily influences how the OS is designed.

At its core, the architecture depends on **how many general-purpose CPUs exist** and **how they interact** with memory, I/O, and each other.

---

## ⚙️ 1.3.1 Single-Processor Systems

### What it is:

- Traditionally, computers had **one CPU with one core** (single execution unit).
- The CPU executes **general-purpose instructions** (user processes, OS code).
- Alongside, there are **special-purpose processors**:
    - Disk controllers (handle scheduling, block transfers)
    - GPU (graphics controller, though older ones were less programmable)
    - Keyboard controllers (scan keystrokes → send scan codes)

👉 These **special-purpose processors** run only **limited instruction sets**, and **do not run OS processes**.

Example:

- A disk controller chip might have its own microcode that manages read/write queues, offloading work from the CPU.
- The CPU doesn’t have to “bit-bang” the disk directly—it just gives high-level commands.

### Important Note:

Even if there are many special-purpose chips, **if there’s only one general-purpose CPU**, it’s still considered a **single-processor system**.

- Today, this is rare. Even cheap smartphones have multiple cores.

---

## ⚙️ 1.3.2 Multiprocessor Systems

### What it is:

- **More than one CPU** capable of executing **general-purpose instructions**.
- CPUs share **system memory** and I/O devices.
- Goal: **throughput increase** → do more in parallel.
    - Increased throughput
    - economy of scale
    - increased reliability

### 🔑 Types of Multiprocessing

### **(a) Symmetric Multiprocessing (SMP)**

- All CPUs are **peers**:
    - Any CPU can run OS code, handle interrupts, or execute user processes.
- Shared resources: memory + I/O bus.
- Each CPU has its own registers and local cache.

Diagram (SMP):

```Plain
   CPU0  <--> L1 cache
   CPU1  <--> L1 cache
     |         |
      \       /
       Shared Bus
         |
       Main Memory
```

👉 Benefits:

- If there are N CPUs, up to N processes can run **concurrently**.
- The OS scheduler distributes processes across CPUs.

👉 Problems:

- Cache coherence (must keep copies in sync).
- Bottlenecks on **system bus** (if too many CPUs share it).

1. Assymetric Multiprocessing — each processor is assigned a specialized task

---

### **(b) Multicore Systems**

- Instead of multiple physical CPUs, a **single chip** has multiple **cores**.
- Faster **on-chip communication** than across a motherboard bus.
- More **power-efficient** than multiple chips.

Example:

- Dual-core CPU: 2 cores, each with its own **L1 cache**, but share an **L2 or L3 cache**.

Diagram (Multicore):

```Plain
 Chip
  ├─ Core0 (Registers + L1 cache)
  ├─ Core1 (Registers + L1 cache)
  └─ Shared L2 cache
       |
     Memory Controller
       |
     Main Memory
```

👉 To the OS, a 4-core chip looks like **4 CPUs**.

  

![[/image 6.png|image 6.png]]

---

### **(c) Non-Uniform Memory Access (NUMA)**

- For scalability, each CPU (or group) has its **own local memory**.
- CPUs are connected with a high-speed **interconnect**. so that all CPUs share one physical address space.

Diagram (NUMA):

```Plain
 CPU0 --- Local Mem0
   |
   | Interconnect
   |
 CPU1 --- Local Mem1
```

👉 Benefit:

- Local memory = very fast (no contention).
    
    👉 Problem:
    
- Remote memory access = slower latency.
- The OS must carefully schedule **processes near their memory** to avoid performance loss.

  

![[/image 1 3.png|image 1 3.png]]

---

### Summary:

- **SMP:** All CPUs share memory (good for small systems).
- **Multicore:** Many cores on one chip (fast, efficient).
- **NUMA:** Distributed memory but shared logically (scales better for large servers).

---

## ⚙️ 1.3.3 Clustered Systems

### What it is:

- Multiple **independent systems (nodes)** connected via **network + shared storage**.
- Each node runs its own OS but **cluster software manages them**.
- Usually sharing storage via a storage-area-network or SAN
- Provides a high availability service which survives failures by adding redundancy.
    - Asymmetric clustering has one machine in hot-standby mode. the hot-standby host machine does nothing but monitor the active server. if that server fails, the hot-standby host becomes the active server.
    - Symmetric clustering has multiple nodes running applications, monitoring each other
- The ability to continue providing service proportional to the level of surviving hardware is called graceful degradation.
- Some systems go beyond it and are called fault tolerant, because they can suffer a failure of any single component and still continue operation.
- Some clusters are for high-performance computing or HPC
    - applications must be written to use parallelization
- Some have distributed lock manager or DLM to avoid conflicting operations.

Diagram:

```Plain
  [Node1: Multicore CPU + OS]   [Node2: Multicore CPU + OS]
             |                              |
             +---- Shared Storage (SAN) ----+
             |                              |
           Cluster Interconnect (LAN/InfiniBand)
```

### 🔑 Features:

- **High availability (HA):**
    - If Node1 fails, Node2 takes over.
    - Used in mission-critical systems (banks, healthcare).
- **Fault tolerance:**
    - Some clusters allow failure of any single component with no downtime.
- **Load balancing:**
    - Work is distributed across nodes.
- **Parallel clusters:**
    - Special applications (e.g., scientific computing, databases) split work across nodes.
    - Example: **Oracle Real Application Clusters** (shared DB access).

---

## ⚡ Key Definitions

- **CPU** – executes instructions.
- **Processor** – physical chip (can have multiple CPUs/cores).
- **Core** – execution unit inside CPU.
- **Multicore** – multiple cores inside a CPU.
- **Multiprocessor** – multiple CPUs (may be multicore).

---

## 📝 Real-World Examples

1. **Single-processor** → 1980s PCs (Intel 8086, Motorola 68000).
2. **SMP multiprocessor** → Early Sun servers with 4 CPUs.
3. **Multicore** → Intel i7 with 8 cores (16 threads with HyperThreading).
4. **NUMA** → AMD EPYC or Intel Xeon scalable servers.
5. **Clustered system** → Google’s data centers, Amazon AWS, Oracle RAC databases.

---

## 🔍 OS Challenges

- **Scheduling:** Distribute processes fairly across CPUs/cores.
- **Synchronization:** Prevent race conditions with shared memory.
- **Cache coherence:** Keep CPU caches consistent.
- **NUMA-aware allocation:** Keep data near the CPU that uses it.
- **Cluster management:** Detect node failures, reroute jobs.

---

✅ That covers the **entire spectrum of system architectures**—from **single CPU** → **multicore SMP** → **NUMA** → **clusters**.

---

Would you like me to also make a **code-level demo** showing how Linux detects multiple CPUs/cores (e.g., parsing `/proc/cpuinfo`), so you see how the OS actually presents SMP/multicore/NUMA to applications?