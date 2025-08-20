Excellent — this section is foundational because it bridges **process scheduling** with **synchronization**. Let’s go step by step, with both theory and practice.

---

## 🔹 1. The Setting: Cooperating Processes and Shared Data

* A **cooperating process** is one that **shares data or resources** with other processes.
* This sharing can happen via:

  1. **Shared memory** (multiple processes read/write the same memory region).
  2. **Message passing** (explicit send/receive communication).

👉 Problem: When processes **concurrently access and modify** shared data, you risk **inconsistency**.

This leads us to **race conditions**:

* A **race condition** occurs when the **final outcome depends on the unpredictable interleaving of instructions** between processes/threads.

---

## 🔹 2. Producer–Consumer Example and Race Condition

We revisit the **bounded buffer (producer–consumer)** problem.

* Shared data: `buffer`, `in`, `out`, and `count`.
* If producer and consumer update `count` concurrently, we may lose updates.

Example:

```c
count++  // implemented as:
register1 = count
register1 = register1 + 1
count = register1

count--  // implemented as:
register2 = count
register2 = register2 - 1
count = register2
```

If these two run interleaved:

1. Producer loads `count=5` into register1.
2. Consumer loads `count=5` into register2.
3. Producer increments → register1=6.
4. Consumer decrements → register2=4.
5. Producer writes back → `count=6`.
6. Consumer writes back → `count=4`.

👉 Result: `count=4`, which is incorrect. The correct answer should have been `count=5`.

That’s the **race condition**.

---

## 🔹 3. The Critical Section Problem

A **critical section** is a code segment where a process accesses shared data.

**Rules we must enforce:**

1. **Mutual Exclusion** → At most one process can be inside its critical section.
2. **Progress** → If no one is inside the critical section, waiting processes must eventually enter (no indefinite postponement).
3. **Bounded Waiting** → Once a process requests entry, there’s a bound on how many others can enter before it.

👉 The challenge: design synchronization mechanisms that enforce these rules, even with **parallel execution and interrupts**.

---

## 🔹 4. Kernel Race Conditions

The **OS kernel itself** faces the same issues:

* Example: updating the list of open files, allocating process IDs, updating memory allocation tables.
* If not synchronized properly, two processes could receive the **same PID** or corrupt internal data structures.

---

## 🔹 5. Naïve Solution: Disable Interrupts

On a **single-core system**:

* Disabling interrupts while inside a critical section guarantees no other process can preempt execution.
* Problem solved, but:

  * It makes the system unresponsive (can’t handle clock interrupts, I/O).
  * Doesn’t scale to multiprocessors (every CPU would need to disable interrupts → slow, inefficient, breaks system timers).

---

## 🔹 6. Preemptive vs Non-Preemptive Kernels

Two broad kernel design strategies:

### (a) **Non-Preemptive Kernel**

* Once a process enters kernel mode, it runs until it blocks, exits, or yields voluntarily.
* Only one kernel process runs at a time → no race conditions inside kernel code.
* ✅ Simpler, safer.
* ❌ Less responsive: a kernel call can “hog” the CPU.

### (b) **Preemptive Kernel**

* Processes can be preempted even inside kernel mode.
* Multiple kernel-mode processes may run simultaneously (especially on multicore).
* ✅ More responsive (better for real-time systems).
* ❌ Harder to design: race conditions can occur, so **locks and synchronization tools** must be used.

👉 Modern OSes (Linux, Windows, macOS) use **preemptive kernels** because responsiveness and real-time support are crucial. But they must carefully implement synchronization (mutexes, semaphores, spinlocks, RCU, etc.).

---

## 🔹 7. Why Synchronization Tools Exist

We need tools to **safely coordinate critical sections**:

* **Hardware support**: atomic instructions (test-and-set, compare-and-swap, memory barriers).
* **Software abstractions**: mutex locks, semaphores, monitors, condition variables.
* **Different tools are better for different contention levels**:

  * Low contention → simple locks are fine.
  * High contention → advanced locks or lock-free algorithms are better.

---

✅ **In summary:**

* The **critical-section problem** is about ensuring correctness when multiple processes/threads share data.
* Naïve fixes like disabling interrupts don’t work well in multiprocessors.
* That’s why we have synchronization primitives (locks, semaphores, monitors).
* The kernel itself is subject to the same problems, which is why **preemptive kernels** require very careful design, especially on SMP systems.

---

Would you like me to next **walk you through the main synchronization tools** (mutexes, semaphores, monitors) with examples — or would you rather dive deeper into the **hardware-level atomic operations** (like compare-and-swap, test-and-set) first?


Perfect — Peterson’s solution is one of those “beautiful but fragile” algorithms in OS theory. It’s extremely useful for learning because it shows **how we can reason about synchronization using only shared variables**, but it also shows **why modern systems require hardware primitives**. Let’s break this down thoroughly.

---

# 🔹 1. The Context

The **critical-section problem** asks us to ensure:

1. **Mutual Exclusion** → only one process in its critical section at a time.
2. **Progress** → if no one is in the critical section, some waiting process must eventually enter.
3. **Bounded Waiting** → once a process requests entry, it won’t be postponed forever.

Peterson’s solution is a **software-only algorithm** that satisfies these three requirements — but **only for two processes**.

---

# 🔹 2. The Algorithm

Two processes: `P0` and `P1`.
Shared variables:

```c
int turn;         // whose turn it is
boolean flag[2];  // intent: does Pi want to enter?
```

Each process `Pi` (where `i` is 0 or 1) executes:

```c
while (true) {
    flag[i] = true;   // I want in
    turn = j;         // but I'll let you go first
    while (flag[j] && turn == j) {
        ; // busy wait
    }

    // ---- critical section ----

    flag[i] = false;  // I’m done
    // remainder section
}
```

---

# 🔹 3. Intuition

* `flag[i] = true` → “I want to enter.”
* `turn = j` → “If you also want to enter, you go first.”
* The **while loop** enforces that both can’t enter simultaneously:

  * If both want to enter, only the one whose turn it isn’t will wait.

So:

* **Mutual Exclusion** → only one enters because `turn` breaks ties.
* **Progress** → if the other isn’t interested (`flag[j] = false`), I can enter immediately.
* **Bounded Waiting** → I may have to wait at most one turn, because once the other leaves, it sets `flag[j] = false`.

It’s elegant — a pure software solution.

---

# 🔹 4. Correctness Proof (Simplified)

1. **Mutual Exclusion**: Both processes cannot be in critical section because that requires `flag[0] = true` and `flag[1] = true` **and** both `turn = 0` and `turn = 1` simultaneously — impossible.
2. **Progress**: If only one process wants in, it enters (since the other’s flag is `false`).
3. **Bounded Waiting**: Once I’m waiting, the other process can only enter once before I get my turn.

So in the abstract model (no instruction reordering, atomic reads/writes), it’s perfect.

---

# 🔹 5. The Problem on Modern Hardware

Peterson’s solution **depends on strict ordering** of `flag` and `turn` assignments.

But modern CPUs and compilers apply **reordering optimizations**:

* CPUs may reorder independent memory operations to improve performance (out-of-order execution).
* Compilers may reorder loads/stores if they see no dependency.
* Caches may delay when writes become visible to other cores.

### Example Problem

Thread 2 executes:

```c
x = 100;
flag = true;
```

CPU/compiler may reorder it as:

```c
flag = true;
x = 100;
```

So another thread may see `flag == true` but `x == 0` — a violation of intended order.

Similarly, in Peterson’s solution:

```c
flag[i] = true;
turn = j;
```

may be reordered as:

```c
turn = j;
flag[i] = true;
```

If this happens for both threads, **both can think it’s their turn** → both enter the critical section simultaneously (see Figure 6.4 in your text).

---

# 🔹 6. Why This Matters

* Peterson’s solution is still **important for teaching** because it shows the **algorithmic reasoning** behind synchronization.
* But in practice, on **modern multiprocessor systems**, it **cannot guarantee correctness** because of memory reordering and weak memory models.
* That’s why **hardware-level primitives** (like `test-and-set`, `compare-and-swap`, memory fences) are required — they enforce atomicity and memory ordering guarantees that software alone cannot.

---

# 🔹 7. Key Takeaways

* Peterson’s solution is **historically important** and **theoretically correct**.
* It enforces mutual exclusion, progress, and bounded waiting — but only with **sequentially consistent memory models** (like on old uniprocessors).
* On modern hardware, you need **atomic hardware instructions + memory fences**.
* That’s why real systems use **locks, semaphores, monitors** built on top of these atomic operations.

---

✅ So in summary:
Peterson’s solution is like a **beautiful proof of concept** — it shows that synchronization is possible with only software and shared variables. But it also shows why we can’t trust software-only approaches on today’s CPUs, and why **hardware support for synchronization** is essential.

---

Do you want me to next walk you through the **hardware primitives** (like test-and-set, compare-and-swap, fetch-and-add) that replaced Peterson’s solution in real systems? That’s the natural continuation of Section 6.4.


Perfect — you’re now at **Section 6.4 (Hardware Support for Synchronization)**, which is the transition away from *pure software-only* solutions (like Peterson’s) into *realistic mechanisms* that modern CPUs actually provide to enforce mutual exclusion and correct ordering of memory operations. Let’s unpack this section step by step:

---

### 🔹 Why hardware support is needed

* Peterson’s solution (and other purely software-based algorithms) **theoretically work**, but **modern CPUs break them** because:

  * CPUs reorder memory operations (to improve performance).
  * Compilers may also reorder independent instructions.
  * Caches and store buffers can delay visibility of writes across cores.
* Result: two threads may *think* they’re synchronized, but they aren’t.

Thus, **hardware primitives** are required to enforce correct synchronization.

---

### 🔹 6.4.1 Memory Barriers (a.k.a. memory fences)

* **Problem:** CPU may reorder instructions like:

  ```c
  x = 100;       // write to x
  flag = true;   // signal ready
  ```

  → It could reorder into `flag=true; x=100;`, breaking correctness.

* **Solution:** Insert a **memory barrier instruction** to say:

  > “All stores before this must be visible before any stores after this.”

* Types:

  * **Load fence**: ensures loads before it are completed before later loads.
  * **Store fence**: ensures stores before it are completed before later stores.
  * **Full fence**: covers both loads and stores.

* Example fixed code:

  ```c
  x = 100;
  memory_barrier();
  flag = true;
  ```

Now, other threads will *never* see `flag=true` without seeing the updated `x`.

👉 These are **low-level**, usually only OS/kernel developers use them directly.

---

### 🔹 6.4.2 Hardware Instructions

Here, the CPU provides **atomic instructions** — operations that can’t be interrupted or interleaved by other threads.

#### (a) **Test-and-Set (TAS)**

```c
boolean test_and_set(boolean *target) {
    boolean rv = *target;
    *target = true;
    return rv;
}
```

* Atomically reads a value and sets it to `true`.
* If `rv` was `false`, the thread “won” the lock.
* If `rv` was `true`, another thread already holds it.

**Lock using TAS:**

```c
while (test_and_set(&lock))
    ;   // spin-wait (busy waiting)
/* critical section */
lock = false;
```

#### (b) **Compare-and-Swap (CAS)**

```c
int compare_and_swap(int *value, int expected, int new_value) {
    int temp = *value;
    if (*value == expected)
        *value = new_value;
    return temp;
}
```

* Atomically: "If `*value == expected`, set it to `new_value`."
* Always returns the *old value*.
* Commonly implemented in hardware (`cmpxchg` on x86 with `lock` prefix).

**Lock using CAS:**

```c
while (compare_and_swap(&lock, 0, 1) != 0)
    ;   // spin
/* critical section */
lock = 0;
```

👉 Both TAS and CAS solve **mutual exclusion**, but:

* They **don’t guarantee bounded waiting** (a thread might starve if unlucky).
* Figure 6.9 shows a modification with `waiting[]` array to enforce fairness.

---

### 🔹 6.4.3 Atomic Variables

* Built on top of CAS (or similar).
* Provide safe, atomic operations on simple data types (`int`, `bool`).
* Example:

  ```c
  void increment(atomic_int *v) {
      int temp;
      do {
          temp = *v;
      } while (temp != compare_and_swap(v, temp, temp+1));
  }
  ```
* Used for counters, reference counts, sequence numbers, etc.
* ⚠️ They don’t solve *all* race conditions — e.g., two consumers may still both think a buffer has an item if they only check `count`.

---

### 🔑 Key takeaways

* **Memory barriers**: prevent reordering issues by forcing visibility of writes/reads.
* **Test-and-Set & CAS**: hardware primitives for atomic locking.
* **Atomic variables**: high-level abstraction for safe updates, built on CAS.
* **Why it matters**: These are the *building blocks* for higher-level synchronization tools (like semaphores, mutexes, condition variables), which are introduced in later sections.

---

👉 Question for you:
Do you want me to also connect this to **how modern locks (like pthread\_mutex in Linux)** are implemented using these primitives, so you see the practical OS-level usage?


Great — now you’re moving from **low-level primitives (Section 6.4)** into **OS-provided synchronization tools (6.5 mutex locks, 6.6 semaphores)**. Let’s break down what’s going on in this passage so it clicks:

---

## 🔹 6.5 Mutex Locks

### What a mutex lock is

* **Mutex = "Mutual Exclusion"**.
* Simplest high-level synchronization primitive.
* You **acquire()** the lock before entering a critical section and **release()** it after leaving.
* Guarantees only one thread is in the critical section at a time.

Pseudo-code:

```c
while (true) {
    acquire(lock);
    critical_section();
    release(lock);
    remainder_section();
}
```

---

### How it works

* Internally, a mutex has a Boolean variable `available`.
* If `available == true`, a thread can acquire it (sets it to false).
* If `available == false`, another thread **must wait**.

---

### Implementation

* Needs **atomic operations** to prevent race conditions (that’s where **CAS** or **test-and-set** from Section 6.4 come in).
* Example with CAS:

```c
acquire() {
    while (compare_and_swap(&available, true, false) == false)
        ;   // spin (busy wait)
}

release() {
    available = true;
}
```

So, a mutex is basically a **software abstraction** built on **hardware atomic instructions**.

---

### Spinlocks vs Blocking Mutex

* The example in the book is a **spinlock**: if a thread can’t acquire the lock, it just **loops (busy waits)**.
* Bad in single-core: wastes CPU cycles while another thread should be running.
* On multicore: spinning can sometimes be okay (one core spins while another finishes the critical section).
* General rule:

  * If the lock will be held for a **very short time** (less than two context switches) → spinlock is fine.
  * Otherwise, you want a **blocking mutex** (the OS puts the thread to sleep instead of spinning).

---

### Lock contention

* **Uncontended lock** → thread acquires immediately (fast).
* **Contended lock** → thread(s) must wait, performance depends on contention level.
* High contention = bad performance because many threads fight for the same lock.

---

## 🔹 6.6 Semaphores

Mutex is the simplest tool. But OSes provide **semaphores**, which are more general.

---

### Semaphore definition

* A **semaphore S** is just an integer with two atomic operations:

  * **wait(S):**

    ```c
    wait(S) {
        while (S <= 0) ;   // busy wait
        S--;
    }
    ```
  * **signal(S):**

    ```c
    signal(S) {
        S++;
    }
    ```
* Rule: Must be **atomic** (no two processes can update S simultaneously).

---

### Two types

1. **Binary semaphore**

   * Values: 0 or 1.
   * Behaves just like a mutex lock.
2. **Counting semaphore**

   * Values: ≥ 0.
   * Used for managing a pool of resources (e.g., 5 printers).
   * Initialized to the number of available resources.
   * Each `wait()` grabs one; each `signal()` releases one.

---

### Usage example (synchronization ordering)

If we want **S2 only after S1**:

* Initialize semaphore `synch = 0`.
* In P1:

  ```c
  S1;
  signal(synch);
  ```
* In P2:

  ```c
  wait(synch);
  S2;
  ```

This ensures P2 waits until P1 finishes S1.

---

### Implementation without busy waiting

* Mutexes in 6.5 (spinlocks) waste CPU cycles.
* Semaphores (properly implemented) **don’t busy wait**:

  * If a process does `wait(S)` and finds `S <= 0`, it **blocks itself (sleep)** and is put in a waiting queue.
  * Another process doing `signal(S)` will **wake up one waiting process**.

This is **much more efficient** than spinlocks.

---

### Key difference from mutexes

* Mutex: only protects critical sections (always binary, only one thread inside).
* Semaphore: more flexible. Can act like a mutex **or** a resource counter.
* Mutexes are usually used for **mutual exclusion**; semaphores are often used for **resource management** and **ordering constraints**.

---

✅ **Summary so far:**

* **Hardware primitives (CAS, TAS, barriers)** → too low-level for programmers.
* **Mutex locks** → OS abstraction built on hardware primitives. Simplest mutual exclusion tool. Spinlocks waste CPU if held long.
* **Semaphores** → generalization (binary = mutex, counting = multiple resources). Can block instead of spin → avoids wasted CPU.

---

👉 Do you want me to also explain **how modern Linux pthread mutexes** are built (they start as spinlocks but switch to blocking mutexes if contention lasts too long)? That connects theory with actual OS design.


Perfect — you’ve now hit **Section 6.7 Monitors**, which is the **next level of abstraction above semaphores**. Let’s unpack it carefully:

---

## 🔹 Why Monitors?

Semaphores are powerful but **error-prone**:

* If you `signal()` before `wait()`, mutual exclusion breaks.
* If you `wait()` twice, you deadlock yourself.
* If you forget a `signal()`, processes may block forever.
* These bugs are **subtle timing errors** — they only show up under certain interleavings.

Monitors were invented to **reduce programmer error** by baking synchronization into the programming language itself.

---

## 🔹 What is a Monitor?

Think of a **monitor** as:

* A **class / module** with:

  * Shared variables (the state).
  * Functions (operations on that state).
* **Only one thread can execute in the monitor at a time**.
  → Mutual exclusion is automatic.

👉 That means **you don’t need to write `wait(mutex)` / `signal(mutex)` yourself**. The compiler/runtime guarantees it.

Pseudo-code:

```c
monitor Account {
    int balance;

    function deposit(int amt) {
        balance += amt;   // only one thread can be here
    }

    function withdraw(int amt) {
        if (balance >= amt)
            balance -= amt;
    }
}
```

If `deposit()` and `withdraw()` run in different threads, the monitor guarantees they **won’t interleave incorrectly**.

---

## 🔹 Condition Variables in Monitors

Mutual exclusion alone isn’t enough — sometimes you need threads to **wait until a condition is true**.

That’s where **condition variables** come in:

```c
condition x;
```

Operations:

* `x.wait()` → suspend this thread until signaled.
* `x.signal()` → wake up one waiting thread (if any).

Example: classic **bounded buffer** producer–consumer

```c
monitor BoundedBuffer {
    condition not_full, not_empty;
    Queue buffer;

    function insert(item) {
        if (buffer.isFull())
            not_full.wait();
        buffer.enqueue(item);
        not_empty.signal();
    }

    function remove() {
        if (buffer.isEmpty())
            not_empty.wait();
        item = buffer.dequeue();
        not_full.signal();
        return item;
    }
}
```

Here:

* Producers wait if buffer is full.
* Consumers wait if buffer is empty.
* No explicit semaphores — just `wait()` and `signal()` inside the monitor.

---

## 🔹 Signal-and-Wait vs Signal-and-Continue

There’s a subtlety:

* When P calls `x.signal()`, and Q is waiting:

  * If both run simultaneously inside the monitor, **mutual exclusion breaks**.
  * Solutions:

    1. **Signal and Wait** → P pauses until Q finishes.
    2. **Signal and Continue** → Q waits until P leaves the monitor.
    3. **Compromise** → P signals and then immediately leaves, letting Q run.

Different languages choose differently (e.g., Java uses signal-and-continue semantics with `wait()`/`notify()`).

---

## 🔹 Implementing a Monitor with Semaphores

Conceptually, monitors are high-level, but underneath they’re built with semaphores:

* A **mutex semaphore** ensures only one thread is in the monitor at once.
* **Condition variables** are implemented with semaphores + counters.
* Example:

  * `wait(x)` → release monitor lock, block on `x`’s semaphore.
  * `signal(x)` → wake up one process waiting on `x`.

So monitors are really **just a safer wrapper around semaphores**, enforced by the compiler/runtime.

---

## 🔹 The Limitations

Even with monitors:

* Programmers can still misuse them:

  * Accessing a resource without calling the monitor.
  * Forgetting to release a resource.
  * Requesting the same resource twice.
* That’s why **Chapter 17 (Access Control)** deals with **enforcing discipline** so you can’t bypass the monitor.

---

## ✅ Quick Comparison

| **Tool**   | **What it is**                                          | **Pros**                                | **Cons**                                     |
| ---------- | ------------------------------------------------------- | --------------------------------------- | -------------------------------------------- |
| Mutex Lock | Simple mutual exclusion                                 | Easy to use                             | Spinlocks waste CPU if long wait             |
| Semaphore  | Counter + wait/signal                                   | General, flexible                       | Easy to misuse (`wait/signal` order bugs)    |
| Monitor    | High-level language construct (like synchronized class) | Mutual exclusion built-in, fewer errors | Still bypassable if programmer ignores rules |

---

👉 So the story is:
**Hardware primitives** (CAS, test-and-set) → **mutexes/semaphores** (OS-level, but error-prone) → **monitors** (language-level, safer).

---

Would you like me to show you how **Java’s `synchronized` + `wait()` / `notify()`** directly implement monitors in practice? That’s usually the “aha moment” because it ties the theory to code you’d actually write.


Great — now you’re at **Section 6.8 Liveness**. This section is basically about what can go wrong when we use synchronization primitives (mutexes, semaphores, monitors, CAS, etc.). Even if you satisfy **mutual exclusion**, you still need to worry about **progress** and **bounded waiting**.

Let’s break it down in detail:

---

# 🔹 What is Liveness?

* **Liveness** = the guarantee that processes will **eventually make progress**.
* A **liveness failure** = when some process **waits forever** (never makes progress).

Examples:

* **Infinite loop** → trivial liveness failure (process never leaves CPU).
* **Busy wait loop** → a process might spin forever if a lock is never released.
* **Deadlock** or **priority inversion** → synchronization-related liveness failures.

This ties back to the **three critical-section requirements**:

1. **Mutual exclusion** → only one at a time.
2. **Progress** → if no one is inside, someone should eventually enter.
3. **Bounded waiting** → no one should wait forever.

Liveness failures violate (2) and (3).

---

# 🔹 Types of Liveness Failures

Two key ones you just read about:

---

## 1. **Deadlock**

* **Definition**: a set of processes is deadlocked if *every process is waiting for an event that only another process in the set can cause*.
* Typically, the event = release of a lock or semaphore.

Classic 2-process, 2-lock example:

```
P0: wait(S); wait(Q); ... signal(Q); signal(S);
P1: wait(Q); wait(S); ... signal(S); signal(Q);
```

Execution order:

1. P0 → `wait(S)` (now holds S).
2. P1 → `wait(Q)` (now holds Q).
3. P0 → tries `wait(Q)` → **blocked** (Q held by P1).
4. P1 → tries `wait(S)` → **blocked** (S held by P0).
   👉 Both wait forever → deadlock.

This is the **circular wait condition** from the classic 4 Coffman conditions (covered in Chapter 8).

---

## 2. **Priority Inversion**

* Happens in **priority-based scheduling systems** (e.g., real-time OS).
* Scenario:

  * Low-priority process **L** holds a lock.
  * High-priority process **H** needs that lock → so H must wait for L.
  * But then a **medium-priority process M** (that doesn’t care about the lock) runs and preempts L.
  * Result: H is indirectly blocked by M (which is absurd — the highest-priority process is waiting for the lowest-priority one).

👉 That’s **priority inversion**.

### Solution: **Priority Inheritance Protocol**

* Temporarily “boost” L’s priority to match H’s.
* L runs immediately, releases the lock quickly.
* Then priority returns to normal.

---

### 🚀 Real-World Example: Mars Pathfinder (1997)

* NASA’s Sojourner rover suffered **resets** because a high-priority task was blocked by a low-priority task holding a semaphore, which in turn was preempted by medium tasks.
* This was a classic case of priority inversion.
* The fix: enable **priority inheritance** in VxWorks (the RTOS). Problem solved on Mars. 🌌

---

# 🔹 Liveness vs Performance

* Even if you avoid deadlock & priority inversion, **performance under contention** matters.
* Section 6.9 evaluates:

  * **CAS-based lock-free approaches** (good under low/moderate contention).
  * **Locks & semaphores** (better under heavy contention).
  * **Atomic operations** (best for simple counters).
  * **Reader–writer locks** (allow concurrency when reads dominate).
  * **Monitors** (high-level, safer, but may add overhead).

---

✅ **Key takeaway**:

* **Deadlock** = everyone waits forever.
* **Priority inversion** = high-priority is indirectly blocked by medium-priority.
* **Liveness** is about guaranteeing that every process eventually makes progress, not just correctness of critical sections.

---

Would you like me to also go ahead and explain **how deadlock is formally modeled and prevented/avoided** (Chapter 8) so you see the connection between this section and what’s coming up?
