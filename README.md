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
# 4.x3=GP (Global Pointer)

- `gp` is a **special register** that points to the global data area in memory.
- Significantly, it points to the **middle of the static/global region**, so both directions (up/down) can be used for different kinds of data.

---

## Why `gp` is Useful

- Programs often need access to global variables, constants, arrays.
- Without `gp`, the CPU needs to **recalculate the address** of each global variable manually every time → slower and more instructions.
- With `gp`, **globals can be accessed quickly with very small offsets**.

---

## Who Sets `gp`

- When a program starts, the **loader/OS sets `gp`** to point to the global data segment.

---

## Additional Notes

- You (the programmer) **rarely set `gp` yourself** — the **compiler and OS handle it**.
- It makes code **position-independent**.
- Using `gp` makes accessing globals **faster**: offsets are **smaller and simpler**.

---

## Summary

- `x3 (gp)` → CPU's **shortcut pointer** to the global/static variable area, so **global data can be accessed quickly with small offsets**.

## 5. x4 = tp (Thread pointer)
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


## 6. x5 t0 (Temporary Register 0)

- `t0` is the first temporary register in RISC-V.
- Temporaries are **caller-saved registers** — meaning if a function wants to keep their value safe, the caller must save them before calling another function.
- Basically, scratch space for quick calculations.
- The CPU needs scratch registers while doing arithmetic, logic, or intermediate steps.
- Instead of using memory (which is slower), CPUs use temporaries to hold temporary values.

### Example:

Suppose you want to calculate: `(a + b) * (c - d)`
1. Store `a + b` in `t0`
2. Store `c - d` in `t1`
3. Multiply them → final result in `a0` (return register)

- If you call another function in between, you **must save `t0` somewhere** (usually on the stack), otherwise, it gets overwritten.

- `t0` (`x5`) **isn't preserved across function calls**.
- If you need long-term storage, use **saved registers**.
- Temporaries = fast & disposable work registers.
  - `x5 (t0)` = CPU's scratch register for temporary calculations.
  - Fast to use, but unsafe across function calls.

---

## 7. x6 t1 (Temporary Register 1)

- `t1` is the second temporary register in RISC-V.
- Belongs to the temporary / caller-saved group (`t0` to `t6`).
- Works just like `t0` (`x5`) — used for quick, short-term storage during calculations.

### Why it exists:
- CPU often needs multiple scratch registers at once.

#### Example:

To calculate expression: `(a + b) * (c - d) + e * f`

- `t0`, `t1`, `t2` can each hold intermediate results **without wasting memory accesses**.

> `t1` = a temp. scratch register used for intermediate results in calculations, very fast but **not preserved across function calls**.

# RISC-V Register Notes

##  X7 → `t2` (Temporary Register 2)

- `t2` is the **third temporary register** in RISC-V.
- Part of the **caller-saved temporaries** (`t0–t6`).
- Works like `t0 (x5)` and `t1 (x6)` → used as a **scratch register** for quick calculations.

###  Why It Exists:
- Programs often need **more than 2 scratch values** at once.
- Avoids **frequent memory access**, which is slow.

###  How It Works:
- You store an intermediate result in `t2`.
- If you call another function in between, its value **might get overwritten** → caller must **save it if needed**.
- It's **fast** but **not safe** across function calls.

###  Usage:
- `t2` is the **last of the**
-  just like t0,t1 t is caller saved
-  Best used when you need multiplee intermediate  results in one calculations (but not preservedacross function calls)


## 9.X8 → s0 / fp (Saved Register 0 / Frame Pointer)

### Dual Role

1. **s0 (Saved Register 0)**  
   - Used as a long-term variable storage across functions.

2. **fp (Frame Pointer)**  
   - Used to mark the base of the current function's stack frame.

### Why it exists

#### As `s0` (Saved Register)
- Some variables must stay alive across function calls.
- If you keep them in temporary registers, they will be lost.
- Saved registers like `s0` are callee-saved — the function you call must restore them before returning.

#### As `fp` (Frame Pointer)
- In functions, local variables are placed in the stack.
- As the stack grows up and down, addresses change.
- `fp` stays fixed at one base of the function's stack frame, especially useful to access variables reliably.
- Important for nested functions and debugging, where stack pointer (`sp`) keeps changing.

### How it works
- When a function starts:
  - The old `fp` is saved onto the stack.
  - The new `fp` is set to the current stack pointer (`sp`).
  - Local variables are accessed using fixed offsets from `fp`.
- When the function ends:
  - Restore the old `fp`.
  - Free the stack.

### Usage
- `s0` acts like any saved register (safe across calls).
- `fp` gives a stable reference point inside the stack frame.
- Not all programs use `fp`, b
### 10.X9 → s1 (Saved Register 1)

- s1 is a saved register  
- Used to store variables that must be preserved across function calls  
- Values in s1 remain unchanged even if function calls happen  
- Useful for keeping data safe when calling other functions  

#### Why it exists:
- When a function calls another function, some registers can get overwritten  
- To avoid losing important values, s1 saves these values  
- This helps in maintaining consistent program state  

#### How it works:
- Before calling a function, current function saves important values in s1  
- Called function can freely use temporary registers without affecting s1  
- After the call, the original function restores values from s1  

#### Imp:
- s1 must be preserved across function calls  
- If a function uses s1, it must save and restore its value properly  
- s1 helps in keeping local variables intact between calls


### 11.X10 → a0 (Argument Register / Return value 0)

- a0 is first argument register  
- It is also the return value register  
- Dual purpose: input (function's first parameter) and output (function's result)

#### Why it exists:
- To avoid slow memory access when passing function inputs/outputs  
- CPU/compiler knows exactly where to look for the first input & where to find the return value  
- This makes function call very fast and consistent

#### How it works:
- When a function is called  
  → The first input goes into a0  
- Inside the function  
  → Calculations may overwrite a0  
- When returning  
  → The final result is placed in a0

#### Imp:
- If function returns 1 value → always in a0  
- If function returns 2 values → a0 = first result, a1 = second result  
- Every function
### 12.X11 → a1 (Argument / Return Register 1)

- a1 is the second argument register  
- Also works as the second return value register (if a function returns two values)

#### Why it exists:
- Most functions need more than one input  
- Instead of getting inputs from memory, RISC-V uses fast registers  
- That’s why a1 is fixed for the second input parameter  
- If the function has multiple outputs, a1 can also carry the second output

#### How it works:
- When function is called  
  → a0 gets first argument  
  → a1 gets the second argument  
- When returning multiple values  
  → a0 → first result  
  → a1 → second result

#### Imp:
- It always holds the second function argument  
- Can also hold the second return value (if needed)  
- Together with a0, a1 is one of the most important registers in function calls

---

### 13.X12 → a2 (Argument Register 2)

- a2 is the third argument register  
- Used to hold the third input parameter of a function

#### Why it exists:
- Most real world functions need more than two inputs  
- Instead of sending inputs via memory, CPU uses fast registers  
- That’s why a2 is fixed for the third input parameter

#### How it works:
- When function is called:  
  → a0 = first argument  
  → a1 = second argument  
  → a2 = third argument  
- Inside the function, a2 can be used directly for calculations  
- On return: a0 (and a1 if needed) carry the outputs → a2 isn’t used for return

#### Imp:
- a2 is only for input arguments, never used as return  
- Together with a0–a7, it allows up to 8 arguments to be passed via registers  
- If function has more than 8 inputs → extra ones are passed via memory/stack

---

### 14.X13 → a3 (Argument Register 3)

- a3 is the fourth argument register  
- It holds the fourth input parameter of a function during a call

#### Why it exists:
- Many functions need 4 or more inputs (like math operations, physics formulas, graphics)  
- To keep things fast, RISC-V assigns fixed registers (a0–a7) for the first 8 inputs  
- a3 is specifically reserved for the fourth input

#### How it works:
- On a function call  
  → a0 = 1st  
  → a1 = 2nd  
  → a2 = 3rd  
  → a3 = 4th  
- Inside the function, a3 is used directly for calculations  
- On return: a3 isn’t used for results

---

### 15.X14 → a4 (Argument Register 4)
    -> a4 is the fifth argument register
    -> It holds the fifth input parameter of a function during a call.

Why it exists:
    -> To avoid slow memory access, RISC-V allows the first 8 inputs to be stored directly in registers (a0-a7).
    -> a4 is dedicated for the fifth argument.
    -> a4 isn't used for return values.

Imp:
    -> a4 = strictly for the fifth input.
    -> not involved in return value.
    -> If the function has fewer than 5 arguments, a4 remains unchanged.
    -> x14 -> register that holds the fifth argument (input) of a function.

*Important*
    x10 (a0) to x17 (a7)
    All of them belong to the argument registers group.

    a5 -> 6th argument
    a6 -> 7th argument
    a7 -> 8th argument

Max 8 arguments via registers. If a function has more than 8 inputs, extra ones are passed using memory (stack).
*/


## (18) X17 -> a7 (Argument Register 7)
- a7 is the eighth argument register.
- Also has a special role: Used to hold the system call number when making OS calls.

Extra Role = tells the OS which system call to perform (example: read, write, exit)

X17 (a7) = the register that carries the eighth argument in a function, and in system calls, it specifies the system call number.

---

## (19) X18 -> S2 (Saved register 2)
- S2 is the first saved register.
- Saved registers (S0 - S11) are meant to hold important values across function calls.
- They are also called callee-saved registers -> the function you call must save and restore them if it uses them.

### How it works:
- Suppose function A calls function B:
  - Function B can freely use temporary registers.
  - But if B touches S2, it must save the old value on the stack & restore it before returning.
- This ensures that S2 still holds the same value when control goes back to function A.

### Imp
- S2 = one of the long-term safe storage registers.
- Used when a function needs to keep a variable intact across calls.
- Compiler automatically manages saving/restoring S2 on stack if needed.

X18 -> A saved register that holds important values safely across function calls (callee must restore if blocked).

---

## Saved Registers
X18 - X27 (S2 - S11)

- These are all saved (callee-saved) registers.
- Their main job = hold variables that need to stay safe across function calls.
- Convention: if a callee (called function) uses them, it must save/restore them.

---

### Functionally,
S2 - S11 are all the same functionally, just a slot where you can keep an important value.

X18 (S2) -> X27 (S11) = all identical roles (just more slots).

---

### Why so many:
- Complex functions sometimes need to keep lots of variables safe.
- Having 10 saved registers (S2 - S11) means compiler/programmer doesn't always have to spill everything into memory.
- Example: recursive functions, loops, big formulas.

---

- All have the same role + safe storage for variables across function calls. No functional difference between them -> they're just extra slots for stability.


###  29) x28 -> t3C Temporary Register 3
    -> t3 is a temporary register
    -> It belongs to t4 - t6 (temporaries)
    -> Used for short-term scratch calculations inside the function.

# Why it exists:
    -> During calculations, CPU often needs extra space to hold intermediate results.
    -> Argument registers (a0 - a7) are reserved for inputs, saved registers (s0 - s11) are supposed to stay stable.
    -> That's why t3 (and other temporaries) are free-use registers.

# How it works:
    -> If a function uses t3, it doesn't save/restore it.
    -> After a function call, the old value in t3 can be overwritten or lost — caller should never expect it to survive.
    -> In other words: use it and forget it.

Imp:
    -> t3 is a caller saved register - caller must NOT depend on its value after a function call.
    -> Best for intermediate steps & scratch work.
    -> No guarantee of stability across function calls.

x28 - x31 -> All are same.
x28 -> mostly used as temp in calculation or loop
x29 -> ""
x30 -> can be used for temp storage that may persist across short sequence
x31 -> often used for larger last temp values in some cases too offset calculations.
*/
<img width="659" height="386" alt="image" src="https://github.com/user-attachments/assets/7a3ad3d5-782e-4245-9121-decc239074e2" />
<img width="581" height="482" alt="image" src="https://github.com/user-attachments/assets/58afda4b-b0fc-458f-96ff-aaed4de46836" />

# it explains how function calling work in the risc v architecture,particularly regarding how arguments are passes and how registers are used in function

# Memory System Architecture

- It refers to the design & organization of a computer memory system, including how memory is structured, accessed, and managed to store & retrieve data efficiently.
- It's a key part of computer architecture because memory speed & organization directly affect a processor's performance.

## Components of Memory System Architecture

A memory system is usually hierarchical, with different levels of memory differing in speed, size, and cost.

1. **Registers:**
   - Fastest memory inside the CPU.
   - Very small in size.
   - Used for immediate operations.

2. **Cache Memory:**
   - Small but fast memory located close to the CPU.
   - Stores frequently used data.
   - Levels: L1 (fastest, smallest), L2 (slightly bigger), L3 (largest, slowest).

3. **Main Memory (RAM):**
   - Larger than cache but slower.
   - Stores programs & data currently in use.

4. **Secondary Storage:**
   - Hard disk drives (HDD), SSDs.
   - Very large but slow.
   - Used for long-term storage.

5. **Tertiary (Off-line) Storage:**
   - Optical discs, tapes, cloud storage.
   - Extremely large & slow.
   - Used for backup/archive.

## Key Concepts in Memory System Architecture

- **Memory Hierarchy**: A memory from fastest/smallest to slowest/largest.
  - Goal: Maximize speed & minimize cost.

- **Access Time**: Time it takes to read/write data.

- **Bandwidth**: Amount of data that can be read/written per unit of time.

- **Latency**: Delay between requesting data & receiving it.

- **Volatility**: Whether memory retains data without power.
  - **RAM** is volatile; **SSD**, **HDD** is non-volatile.
  - Retains data even when there is no pow
 
    <img width="613" height="481" alt="image" src="https://github.com/user-attachments/assets/7f0d63fd-5a2e-4e9c-ac0a-7b196aea5b90" />

# Virtual Memory System
**What Is Virtual Memory?**
Virtual memory creates an illusion that each program has access to a large, private
memory space, even though physical memory is limited and shared.
