  

Main memory - only large storage media that the CPU can access directly

- Random access
- typically volatile
- typically random-access memory in the form of Dynamic Random-access Memory or DRAM

Secondary storage - extension of main memory that provides large nonvolatile storage capacity.

- HDDs - rigid metal or glass platters covered with magnetic recording material
    - disk surface is logically divided into tracks, which are subdivided into sectors
    - the disk controller determines the logical interaction between the device and the computer
- NVM or Non-volatile memory devices - are faster than hard disks, nonvolatile
    - various technologies
    - becoming more popular as capacity and performance increases, price drops

  

  

## **1. The Storage Hierarchy — The “Pyramid” of Memory**

From fastest & smallest to slowest & largest:

1. **Registers** – Inside CPU, store data for immediate operations.
2. **Cache** – Very fast memory close to CPU, stores frequently used data.
3. **Main Memory (RAM / DRAM)** – Holds running programs and data.
4. **Nonvolatile Memory (NVM)** – Flash storage, SSD.
5. **Secondary Storage (HDD, optical disk)** – Larger, slower, permanent storage.
6. **Tertiary Storage** – Archival storage (magnetic tape, Blu-ray).

💡 **Trade-off**: The faster the memory, the more expensive it is per byte — so we use a mix.

---

## **2. Volatile vs Nonvolatile**

- **Volatile**: Data lost when power is off (RAM, CPU cache, registers).
- **Nonvolatile**: Data persists without power (SSD, HDD, EEPROM, flash).

---

## **3. How the CPU Uses Storage**

In a **von Neumann architecture**, the CPU executes this cycle:

1. **Fetch** instruction from main memory into instruction register.
2. **Decode** instruction.
3. **Fetch operands** (data) if needed.
4. **Execute** operation.
5. **Store** results back to memory.

The CPU doesn’t care _how_ an address is generated — it just works with a sequence of addresses.

---

## **4. Example: Simulating Memory and Registers in C**

Here’s a simplified model of how registers and memory interact:

```C
\#include <stdio.h>
\#include <string.h>

\#define MEMORY_SIZE 1024 // 1KB RAM

unsigned char memory[MEMORY_SIZE]; // main memory
unsigned int registerA = 0;         // CPU register

// Simulate "load" instruction
void load(int address) {
    registerA = memory[address];
    printf("Loaded %u from memory[%d] into registerA\n", registerA, address);
}

// Simulate "store" instruction
void store(int address) {
    memory[address] = registerA;
    printf("Stored %u from registerA into memory[%d]\n", registerA, address);
}

int main() {
    // Example: store 42 into memory[10]
    registerA = 42;
    store(10);

    // Load it back into registerA
    registerA = 0;
    load(10);

    return 0;
}
```

**What this models:**

- `registerA` = CPU register.
- `memory[]` = DRAM / RAM.
- `store()` & `load()` = basic CPU instructions.

---

## **5. Example: File as Secondary Storage in C**

Here’s a way to treat a file as persistent storage (like an SSD/HDD):

```C
\#include <stdio.h>

int main() {
    FILE *disk = fopen("disk.bin", "wb+"); // open for read/write binary
    if (!disk) return 1;

    // Write data to "disk" (secondary storage)
    int data = 1234;
    fwrite(&data, sizeof(int), 1, disk);

    // Move to beginning and read it back
    rewind(disk);
    int readData;
    fread(&readData, sizeof(int), 1, disk);

    printf("Read from disk: %d\n", readData);

    fclose(disk);
    return 0;
}
```

This mimics how secondary storage holds data permanently until read back into memory.

---

## **6. Real-World OS Perspective**

- **Registers** are manipulated by CPU instructions directly.
- **RAM** is accessed via **load/store** instructions.
- **Secondary storage** is accessed **through the OS’s I/O system calls** (e.g., `read()`, `write()` in Linux).
- The OS moves data between these layers using **buffers** and sometimes **DMA** (Direct Memory Access) to speed things up.

---

## **7. Visual Diagram**

```Plain
[Registers] - nanoseconds
   ↑
[Cache] - ns
   ↑
[RAM / DRAM] - microseconds
   ↑
[SSD / NVM] - 100s of microseconds
   ↑
[HDD / Secondary] - milliseconds
   ↑
[Tertiary Storage] - seconds-minutes
```

Closer to the CPU → faster, smaller, more expensive.

---

![[/image 4.png|image 4.png]]