  

> [!important] **Microarchitecture** of a processor — is the underlying system design by which a processor executes instructions.

- One of the remarkable feats of microprocessors is that they employ complex and exotic microarchitectures, in which instructions can be executed in parallel, while presenting an operational view of simple sequential instruction execution.

  

There are two different lower bounds that characterize the maximum performance of a program.

- The latency bound is encountered when a series of operations must be performed in strict sequence, because the result of one operation is required before the next one can begin.
    - This bound can limit program performance when the data dependencies in the code limit the ability of the processor to exploit instruction-level parallelism.
- The throughput bound characterizes the raw computing capacity of the processor’s functional units. This bound becomes the ultimate limit on program performance.

  

For example, Intel processors are usually described in the industry as **superscalar**—meaning that they can perform **multiple operations on every clock cycle** and **out of order,** meaning that the order in which instructions execute need not correspond to their ordering in the machine-level program.

  

The overall design has two main parts:

- The instruction control unit or ICU which is responsible for reading a sequence of instructions from memory and generating from these a set of primitive operations to perform on program data
- The execution unit or EU which then execute these operations.