# RISC--V
#PROCESSORS

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

 # Instruction Execution Cycle

## 1. What is it?
The **Instruction Execution Cycle** (also called **Fetch–Decode–Execute Cycle**) is the **step-by-step process followed by the CPU to execute each instruction** in a program.  
It is a **repeating loop** that continues until the program ends.

---

## 2. Main Steps

### (a) **Fetch**
- CPU gets (fetches) the instruction from **memory (RAM)**.
- The **Program Counter (PC)** holds the address of the next instruction.
- Instruction is copied into the **Instruction Register (IR)**.
- **PC** is updated to point to the next instruction.

### (b) **Decode**
- CPU **decodes (interprets)** the instruction in the **IR**.
- The **Control Unit (CU)** figures out:
  - What operation to perform (**add, move, jump, etc.**).
  - Which **operands (data/addresses)** are needed.

### (c) **Execute**
- CPU carries out the instruction:
  - If it’s **arithmetic** → **ALU** performs the operation.
  - If it’s **memory access** → data is read/written.
  - If it’s a **jump/branch** → **PC** is updated to a new address.

### (d) **Store (sometimes added)**
- Result of the operation is **stored back into memory or a register**.

## 3. Cycle Repeats
- After execution, the CPU goes back to **fetch the next instruction**.
- This cycle continues until the program ends.

[Fetch] → [Decode] → [Execute] → [Store] → (back to Fetch next instruction)

<img width="648" height="419" alt="image" src="https://github.com/user-attachments/assets/5c0e2301-bc7f-46a2-98c0-1a7f39ebb44f" />

# UNDERSTANDING BINARY AND DATA REPRESENTATION
<img width="632" height="400" alt="image" src="https://github.com/user-attachments/assets/f1383251-f67c-4aff-bfdf-29e4c85e6a4c" />
<img width="656" height="237" alt="image" src="https://github.com/user-attachments/assets/52b75c03-4ca9-4294-9a44-827341b21643" />

# Registers  :THE PROCESSOR'S WORKSPACE

## 1. **Definition**  
- **Registers** are **very small, very fast storage locations inside the CPU**.  
- They hold data, instructions, addresses, or control information temporarily **while the CPU is working**.  
- Think of them as the CPU’s **scratchpad** or **notepad** 📝.  

---

## 2. **Why are they needed?**  
- Accessing **RAM (memory)** is **slow** compared to CPU speed.  
- Registers are built **directly inside the processor chip**, so they can be accessed in **1 CPU clock cycle**.  
- This makes execution **much faster**.  

---

## 4. **Analogy**  
- Imagine you are solving math on paper:  
  - **Brain = CPU** 🧠  
  - **Paper = RAM (memory)** 📄  
  - **Numbers you keep in your head = Registers**  

➡️ Registers are small but **super fast** → that’s why the CPU loves to keep frequently used values there.  

<img width="588" height="343" alt="image" src="https://github.com/user-attachments/assets/19e07fab-91d3-4a67-ae09-053ad5556465" />
**Each flipflop stores 1 bit(0 or 1)
Together,32 flipflops=32 bit = 1 full register**
<img width="664" height="493" alt="image" src="https://github.com/user-attachments/assets/d16e3113-32bb-4bb3-b365-debdff283c73" />
# RISC-V Registers Notes

## 1. x0
- In RISC-V, x0 is hardwired to 0  
  → Means it always contains 0 – you cannot overwrite it.  
  → Any instruction that tries to write to x0 will be ignored.  

- It makes programming simpler & faster because CPU has a free zero constant available.  

### Common Use
- Clearing a register  
- Copying values  
- Comparisons  
- Creating NOP  

- Saves hardware complexity → no need to load 0 from memory  
- Saves register space → instead of unrolling/occupying a general register by storing 0 in it, dedicate the register that is always 0  
- Makes assembly code shorter & more efficient  
### Common Uses
```asm
add x5, x6, x0     # Copy value of x6 into x5
add x7, x0, x0     # Clear register (x7 = 0)
beq x3, x0, label  # Compare x3 with 0
```
## 2. x1 = ra (Return address register)
- When you call a function, CPU must remember where to come back once that function finishes.  
- It is a special address register that holds that return address.  

### Instruction: `jal` (Jump and Link)
- Used for function calls.  
- It does two things:  
  (a) Jumps to the function  
  (b) Stores the address of the next instruction (i.e. return address) into `ra (x1)`  

- So without the `ra` register, CPU wouldn’t know where to go back.  
- Every time a function is called, the return point is saved into `ra`.  

### Common Use
- Function calls  
- Nested calls  
  - `ra` stores return address, but if another function is called inside, the old one must be saved on stack, otherwise return path might be lost  

---

### Important
- If you overwrite `ra` accidentally → program will jump to the wrong place = crash.  
- That is why compiler saves `ra` on stack if multiple nested calls are happening.  
  

# RISC-V Registers Notes

## 3. x2 = sp (Stack pointer)
- It always points to the top of the stack in memory.  
- The stack is a special memory area used during function calls.  

### CPU uses stack to manage:
1. Local variables inside a function  
2. Return addresses  
3. Saved registers  

---

### How it works
- **When a function starts:**  
  - `sp` moves down (lower memory) to make space.  
  - Local variables and saved registers are stored there.  

- **When function ends:**  
  - Data is removed.  
  - `sp` moves back up to its old position.  

- Without stack pointer, CPU won’t know where local variables or `ra` are kept.  

- Helps in nested functions & recursion.  
- Keeps each function’s workspace separate.  

---

### Important
- `sp` must always be aligned properly (usually multiple of 16).  
- Compiler automatically uses `sp` when you work with functions.  
- If `sp` gets corrupted → program crashes because CPU won’t find correct return address.  

---

### In short
`x2 (sp)` = CPU’s **bookmark for stack** → it always points to the current top of the stack so that function calls, local variables & saved registers are managed safely.  
# RISC-V Registers Notes
here
## 4. x4 = tp (Thread pointer)
- It is a special register used in multi-threaded programs.  
- Each thread (like a mini-program running inside your program) needs its own private data area.  
- `tp` points to that thread’s local storage (TLS).  

---

### In modern programs (especially OS, servers, apps)
- Many threads run at the same time.  
- Each thread must keep:  
  - Its own stack  
  - Its own local/global variables  
  - Its own runtime data  

- Without `tp`, CPU wouldn’t know which thread’s data it is using → chaos.  
- With `tp`, each thread can quickly access its private variables.  

---

### Important
- Normal single-threaded programs → don’t use `tp`.  
- In OS kernels, multithreading libraries & browsers `tp` is essential.  
- Just like `gp` makes global data access fast, `tp` makes thread-local data access fast.  

# 5MEMORY SYSTEM ARCHITECTURE

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


