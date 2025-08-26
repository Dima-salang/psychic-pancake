# Design and Implementation

- design and implementation of OS is not “solvable”, but some approaches have proven successful.
- the internal structure of different OSes can vary widely
- we start the design by defining goals and specifications
- affected by choice of hardware and the type of system.

  

  

# **1. Design Goals**

When building an OS, you have two perspectives:

- **User goals**
    - Easy to use
    - Fast / efficient
    - Reliable (doesn’t crash, doesn’t lose data)
    - Secure (keeps processes isolated, protects memory)
- **System (developer) goals**
    - Easy to maintain and extend
    - Portable across hardware
    - Flexible (can adapt to new policies like new schedulers, file systems)
    - Efficient in resource usage

👉 Example contrast:

- **VxWorks (RTOS)** → designed for embedded devices, hard deadlines, deterministic scheduling.
- **Windows Server** → designed for enterprise apps, throughput, rich APIs, multi-user support.

---

# **2. Mechanisms vs. Policies**

This is one of the most fundamental OS design principles.

- **Mechanism** = _how_ something is done (low-level plumbing).
- **Policy** = _what decision_ is made (high-level strategy).

### Example:

- **Mechanism:** Timer interrupt hardware + handler (triggers after N ms).
- **Policy:** Do we use this to run round-robin scheduling with a 10ms quantum, or do we favor high-priority tasks first?

### Why separate them?

Because **policies change often** (business needs, workload types) while mechanisms are harder to change (they’re in the kernel or even hardware).

👉 Example in the real world:

- **Linux:** Scheduler mechanism is modular. The default is CFS (Completely Fair Scheduler), but you can replace it with a real-time scheduler or experimental ones by recompiling.
- **Windows/macOS:** Scheduler mechanism and policies are tightly bound into the kernel → ensures consistent “look and feel” but less flexible.

### Code Sketch — Mechanism vs Policy

```C
// Mechanism: Timer interrupt handler
void timer_interrupt_handler() {
    // Mechanism: signal the scheduler
    scheduler_tick();
}

// Policy: Scheduling decision
process_t* scheduler_pick_next() {
    // Example policy: Round Robin
    return dequeue_ready_queue();
}
```

---

# **3. Implementation**

Historically:

- **Old OSes (1960s–70s)** → Assembly only (UNIX v1, IBM OS/360).
- **Modern OSes** → Mostly C, with some assembly (context switching, bootstrapping, CPU instructions).
- **High-level parts** → C++ (Windows kernel), Java (Android framework), Rust (emerging in Linux drivers).

### Example: Android

- **Kernel:** C + some assembly (Linux base).
- **System libraries:** C/C++.
- **Frameworks & APIs:** Mostly Java.
- **UI/Apps:** Java/Kotlin.

---

# **4. Performance Considerations**

- Writing in **C** instead of assembly makes the OS portable and maintainable.
- Bottlenecks are usually in:
    - **Interrupt handlers**
    - **Memory manager**
    - **I/O subsystem**
    - **Scheduler**

These may be tuned or even written in assembly for speed.

### Example: Context Switch in Assembly (x86)

```Assembly
; Save current process state
pushad                ; save registers
mov eax, [current]    ; get current PCB
mov [eax].esp, esp    ; save stack pointer

; Switch to next process
mov eax, [next]
mov esp, [eax].esp    ; restore stack pointer
mov [current], eax    ; update current

popad                 ; restore registers
iret                  ; return to new process
```

---

# **5. Key Takeaways**

- **Design goals** split between what users want and what developers want.
- **Mechanisms vs policies** is critical: OSes should provide flexible mechanisms so policies can evolve.
- **Implementation** has shifted from assembly → C/C++ → (increasingly) Rust and higher-level languages.
- **Performance** comes less from hand-tuned assembly and more from good algorithms and data structures.

---

✅ So to connect this to your earlier question about timers and scheduling:

- The **timer interrupt mechanism** is always there.
- The **policy** (round robin, CFS, priority scheduling) decides what to do at each tick.
- The **implementation** is in C (kernel), with tiny slices of assembly (context switch).

---

Would you like me to also show you a **real Linux kernel example** of how a timer tick hooks into the scheduler (with simplified code), so you can see mechanisms and policies in action?