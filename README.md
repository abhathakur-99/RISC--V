# RISC--V
PROCESSORS

A **processor** (CPU – Central Processing Unit) is the **brain of a computer**, responsible for executing instructions, performing arithmetic and logical operations, and controlling the flow of data between memory and peripherals.
<img width="2560" height="1600" alt="image" src="https://github.com/user-attachments/assets/5793a300-e132-4c01-91b0-6b822f524d85" />

### **Key Components of a Processor**

1. **ALU (Arithmetic Logic Unit)**

   * Performs **integer arithmetic** (add, sub, multiply, divide) and **logic operations** (AND, OR, XOR).

2. **FPU (Floating Point Unit)**

   * Handles **floating-point operations** like decimal calculations and scientific computations.

3. **Registers**

   * Small, fast storage locations inside the CPU for temporary data and instructions.

4. **Control Unit (CU)**

   * Directs the **flow of instructions and data** between ALU, memory, and I/O devices.

5. **Instruction Decoder**

   * Translates machine instructions into **control signals** for the ALU, FPU, memory, and other components.

6. **Cache (Optional)**

   * High-speed memory inside or near the CPU for **quick access to frequently used data**.

7. **Buses**

   * Electrical pathways that **transfer data, instructions, and addresses** between CPU, memory, and peripherals.

A processor (CPU) is the brain of a computer. It executes instructions, performs arithmetic & logic operations, handles floating-point calculations via the FPU, and controls data flow. Key components include ALU, FPU, registers, control unit, instruction decoder, cache, and buses.


5 MEMORY SYSTEM ARCHITECTURE
The term “Memory System Architecture” refers to the design and organization of a computer’s memory system, including how memory is structured, accessed, and managed to store and retrieve data efficiently. It’s a key part of computer architecture because memory speed and organization directly affect a processor’s performance

1. Components of a Memory System Architecture

A memory system is usually hierarchical, with different levels of memory differing in speed, size, and cost:

Registers

Fastest memory inside the CPU.

Very small in size.

Used for immediate operations.

Cache Memory

Small but faster memory located close to the CPU.

Stores frequently used data.

Levels: L1 (fastest, smallest), L2 (slightly bigger), L3 (largest, slower).

Main Memory (RAM)

Larger than cache but slower.

Stores programs and data currently in use.

Secondary Storage

Hard disk drives (HDD), SSDs.

Very large but slow.

Used for long-term storage.

Tertiary/Off-line Storage

Optical disks, tapes, cloud storage.

Extremely large and slow.

Used for backup/archive.

2. Key Concepts in Memory System Architecture

Memory Hierarchy: Organizes memory from fastest/smallest to slowest/largest. Goal: maximize speed and minimize cost.

Access Time: Time it takes to read/write data.

Bandwidth: Amount of data that can be read/written per unit time.

Latency: Delay between requesting data and receiving it.

Volatility: Whether memory retains data without power (RAM is volatile, SSD/HDD is non-volatile).

3. How It Works Together

The CPU first checks registers → then cache → then main memory → then secondary storage if data isn’t found.

Techniques like caching, paging, and virtual memory help manage data efficiently across this hierarchy.


