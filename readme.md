# CS-301 - Week 9 - Chapter 6: A Computing Machine (Part 2)

**Focus:** Actually *using* TOY like a real computer, and seeing how virtual machines (like the JVM) fit into the bigger picture.

This week finishes Chapter 6 by:

- Writing and reading TOY machine language programs  
- Using a TOY simulator (a virtual TOY machine written in Java)  
- Connecting TOY to the Java Virtual Machine and the idea of virtual machines in general  

Also: **no quiz this week**, so you can shift more attention to Final prep.

---

## 1. Reading Checklist

### Required Textbook Reading

From *Computer Science: An Interdisciplinary Approach* (Sedgewick & Wayne):

---

#### 6.3 Machine-Language Programming (TOY Programming)

Focus on how to actually *program* TOY:

- **What a TOY program looks like**
  - Each instruction is a 16-bit word written as 4 hex digits.
  - Often shown in tables with: address, instruction (hex), and a comment.
  - Memory layout:
    - Low addresses hold instructions.
    - Higher addresses can hold data, arrays, constants.

- **Variables and assignment**
  - Variables live in registers or in memory.
  - Typical patterns:
    - Load constant into register: use a load instruction that picks up a constant from memory.
    - Move data between registers using add with register 0 (which is always 0).

- **Conditionals (if statements)**
  - There is no explicit high-level `if` instruction. Instead you:
    1. Compute a comparison result in a register (for example, subtract two values).
    2. Use a branch instruction that jumps if a register is positive, zero, or negative.
    3. Arrange labels so that you either skip a block or jump into it based on the condition.
  - Conceptually:
    - High-level `if (cond) { body }` turns into
      - evaluate cond into a register
      - branch to `end` if condition is false
      - execute body
      - `end:` continue

- **Loops**
  - Loops are made from:
    - A label at the top of the loop.
    - A body of instructions.
    - A branch at the bottom that jumps back to the label if the condition is still true.
  - Conceptually:
    - High-level `while (cond) { body }` becomes:
      - `top:` evaluate cond
      - if false, branch to `done`
      - body
      - branch to `top`
      - `done:`

- **Arrays and memory addressing**
  - Arrays live in contiguous memory locations.
  - You keep a base address in a register, and an index in another register.
  - Effective address pattern: `address = base + index`.
  - Typical steps:
    1. Put base address of array in `R1`.
    2. Put index (or loop counter) in `R2`.
    3. Compute `R3 = R1 + R2` to get the memory address of `array[R2]`.
    4. Load from `R3` or store to `R3`.

- **Input and output**
  - TOY uses specific memory addresses or instructions for standard input and output.
  - You do not need to memorize the exact addresses for this class, but understand:
    - There are instructions that read a value into a register.
    - There are instructions that print a register value.

- **Functions and call/return discipline (high level)**
  - There is no special hardware stack in TOY.
  - The section shows patterns for:
    - Passing arguments in registers.
    - Reserving a register (or memory location) for a return address.
    - Jumping to a label that acts like a function entry.
    - Using a jump back to return.
  - You do not need to remember every detail of their calling convention, but you should:
    - Recognize that you can emulate function calls using jumps and registers.
    - See that high-level `call f(x)` is just a pattern of TOY instructions.

**What you should be comfortable doing from 6.3:**

- Read a short TOY program and:
  - Trace the values of PC, registers, and key memory locations.
  - Explain what the program computes in plain English.
- Given a simple description like “add all numbers in this small array,” sketch a TOY-level solution:
  - Initialize indices and counters.
  - Loop and update PC, index, and sum.
- Recognize which part of a TOY program corresponds to:
  - A loop.
  - A conditional.
  - A variable update.
  - A simple array access.

---

#### 6.4 TOY Virtual Machine

This section is about running TOY on top of Java:

- **Simulators**
  - Instead of building physical hardware, you can write a program that simulates the behavior of a machine.
  - Advantages:
    - Easier to change (add opcodes, change debugging tools).
    - Much cheaper and faster to iterate on.
  - You can use simulators to:
    - Prototype new architectures.
    - Keep old programs alive on new hardware.

- **TOY virtual machine data structures**
  - The Java simulator uses:
    - `int[] reg` for the 16 registers.
    - `int[] mem` for the 256 memory words.
    - `int pc` for the program counter.
    - Possibly an `int ir` (instruction register) and flags or a boolean `running`.
  - Programs are loaded into `mem` as 16-bit integers that correspond to the TOY instruction encoding.

- **Main simulation loop (fetch-decode-execute in Java)**

Conceptual pseudocode:

```java
while (running) {
    // fetch
    ir = mem[pc];

    // increment PC
    pc = (pc + 1) & 0xFF;  // keep it in 0..255

    // decode
    int opcode = (ir >> 12) & 0xF;
    int d      = (ir >> 8)  & 0xF;
    int s      = (ir >> 4)  & 0xF;
    int t      =  ir        & 0xF;

    // execute
    switch (opcode) {
        case 0: /* halt or no-op */ break;
        case 1: /* add, reg[d] = reg[s] + reg[t] */ break;
        // more opcodes...
    }
}
```

You do not need to memorize the exact code, but you should recognize:

- Each step matches the TOY hardware cycle:
  - Fetch from memory at PC.
  - Increment PC.
  - Decode the 16-bit instruction into opcode and operand fields.
  - Execute the corresponding behavior, which might:
    - Change registers.
    - Change memory.
    - Overwrite PC for jumps and branches.
- This is exactly the same conceptual loop you saw in 6.2, just written in Java.

- **Relationship to the JVM**
  - TOY simulator is a simple example of a virtual machine.
  - The Java Virtual Machine:
    - Reads Java bytecode instead of TOY instructions.
    - Has many more instructions and features.
    - Uses a stack-based model rather than TOY's register-based model.
  - Idea: machines, interpreters, and virtual machines can all be expressed as programs running on another machine.

**What you should be comfortable doing from 6.4:**

- Describe in English what a simulator does:
  - It takes a description of a program in some instruction set and emulates that instruction set.
- Sketch the fetch-decode-execute loop in pseudocode.
- Explain how TOY programs can keep running on new hardware if:
  - You recompile or rewrite the simulator for the new hardware.
  - You do not have to rewrite all your TOY programs themselves.
- Recognize that JVM is essentially a complex, production-grade version of the same idea.

---

### Companion Website

Start from the chapter hub (same as last week):

- Chapter hub: <https://introcs.cs.princeton.edu/java/60machine/>

Then drill into:

- **6.3 TOY Programming**  
  <https://introcs.cs.princeton.edu/java/63programming/>

- **6.4 TOY Virtual Machine**  
  <https://introcs.cs.princeton.edu/java/64simulator/>

For each of 6.3 and 6.4:

- Read the main text.
- Scroll to the bottom and at least skim:
  - **Q + A** for subtle points and clarifications.
  - **Exercises** for small, concrete TOY problems.
- Look at any downloadable `.toy` programs or sample input files; keep a few handy in your repo.

---

## 2. No Quiz This Week

There is **no Week 9 quiz**.

Use that time to:

- Revisit earlier quiz questions that felt shaky.  
- Re-work any conversions, two’s complement, or TOY questions that you got wrong the first time.  
- Practice stepping through TOY code and predicting what PC, registers, and memory will look like after a few steps.  

---

## 3. Final Exam Prep Guide

The Final is basically a “greatest hits” compilation of this quarter’s quizzes.

### 3.1 What the Final Will Look Like

- The Final Exam pulls from **quiz questions you have already seen**:
  - Expect familiar multiple choice questions (possibly re-ordered or with small tweaks).
  - Expect programming and reasoning problems in the same style as prior work.
- You get **one attempt only** on the Final (no retakes like the quizzes).
- The Final is **asynchronous during Finals Week**:
  - Available all Finals Week.
  - You choose when to start it.
  - Once you start, the clock and one-shot attempt rules apply.

### 3.2 Using Quizzes as a Study Bank

Because your instructor keeps the **highest grade per quiz**:

- You can **safely re-take** quizzes:
  - Figure out missed multiple-choice questions.
  - Confirm which answers are correct.  
  - Your score only goes **up or stays the same**, never down.
- Strategy:
  1. Open your quiz attempts and identify questions you missed.
  2. Re-work them using:
     - The textbook sections.
     - Your weekly READMEs and notes.
     - The companion website examples.
  3. Re-take the quiz once you are confident, just to lock in the mental model.

### 3.3 Screen Recording Requirements

For the Final Exam:

- You will **record your screen** to demonstrate you are not using unauthorized resources.  
- Programming questions can be more time consuming, so:
  - Your instructor allows **separate recordings** for each programming task.
  - That means you do not have to make one huge, single recording.
- Before Finals Week:
  - Make sure you know how to:
    - Start and stop your chosen screen recording tool.
    - Save and access the recordings.
    - Keep only allowed windows and resources open while recording.

---

## 4. TOY Practice Ideas For This Week

Here are some concrete things you can do while you read 6.3 and 6.4.

### 4.1 Hand-tracing TOY Programs

Pick a small TOY program from 6.3 or the companion site and:

1. Write down:
   - PC.
   - Contents of each register.
   - Any relevant memory locations.
2. Step through one instruction at a time:
   - Update PC according to the instruction (increment or branch).
   - Update any register or memory locations affected.
3. Keep a short execution trace:
   - A table with rows like: step number, PC, instruction, changed registers, changed memory.

Key skills:

- Reading hex opcodes and decoding opcode and operands in your head.  
- Seeing how branches implement loops and conditionals.  

### 4.2 Run the Java TOY Simulator

On the 6.4 page, look at `TOY.java` (the simulator) and:

- Notice how it:
  - Stores registers in an `int[]`.
  - Stores memory in an `int[]`.
  - Uses a single `int pc` to track the program counter.
- If you get ambitious:
  - Download `TOY.java` and compile it.
  - Run one of the sample `.toy` programs.
  - Compare the simulator’s behavior to your hand-traced expectations.

You do not have to deeply understand the entire implementation code, but it is helpful to see how a virtual machine is just a normal Java program with a loop.

### 4.3 Connect TOY to the JVM Conceptually

As you read, keep these parallels in the back of your mind:

- **TOY machine**  
  - Simple ISA, small instruction set, 16 registers, 256 memory words.
  - Physical or imagined hardware, plus a simulator.

- **TOY virtual machine (simulator)**  
  - Java code that treats TOY registers and memory as arrays.
  - A big loop that:
    - Fetches the next instruction.
    - Decodes opcode and operands.
    - Executes behavior.

- **Java Virtual Machine (JVM)**  
  - Much larger instruction set, designed for running compiled Java bytecode.  
  - Same core idea: a virtual computer that you simulate on real hardware.  

If you understand TOY plus TOY’s simulator, you have the mental model you need for the JVM.

---

## 5. Repo Notes (for Future Me)

Suggested structure for the Week 9 repo:

- `toy-programs/`  
  - `sum.toy` - example that sums a few numbers.  
  - `max.toy` - finds the maximum of a small list in memory.  
  - `countdown.toy` - simple loop and branch example.  

- `src/`  
  - `ToyNotes.java` - a Java file where you jot down comments and experiments while reading `TOY.java`.  
  - `ToyTraceHelper.java` (optional) - maybe a helper that prints out traces or decodes instructions in a more human readable way.  

- `notes/`  
  - `week9-notes.md` - narrative notes on:
    - How TOY instructions map to higher-level patterns (if, while, for).
    - How TOY simulator is structured.
    - How TOY compares to the JVM.

- `exam-prep/`  
  - `quiz-review.md` - list of quizzes and topics to revisit.  
  - Screenshots or text of any questions you initially missed and later fixed.

The goal is that Future You can open Week 9 and immediately see:  
“Ah, this is where we actually programmed TOY and connected it to virtual machines, and this is where Final prep started in earnest.”
