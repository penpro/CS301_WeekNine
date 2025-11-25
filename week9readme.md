# CS-301 - Week 9 - Chapter 6: A Computing Machine (Part 2)

**Focus:** Actually *using* TOY like a real computer, and seeing how virtual machines (like the JVM) fit into the bigger picture.

This week finishes Chapter 6 by:

- Writing and reading TOY machine language programs  
- Using a TOY simulator (a virtual TOY machine written in Java)  
- Connecting TOY to the Java Virtual Machine and the idea of virtual machines in general  

Also: **no quiz this week** so you can shift more attention to Final prep.

---

## 1. Reading Checklist

### Required Textbook Reading

From *Computer Science: An Interdisciplinary Approach* (Sedgewick & Wayne):

- **6.3 TOY Programming / Machine-Language Programming**  
  Focus on:
  - How TOY programs are written in hex and loaded into memory
  - Tracing TOY execution step by step (PC, registers, memory)
  - Using basic instruction patterns to build:
    - Conditionals (if / branch)
    - Loops (using branches and labels)
    - Simple algorithms like “counting,” “searching,” or “testing properties” of numbers
  - The idea that TOY is *universal* in the same sense as Java: given enough memory and time, it can compute anything Java can

- **6.4 TOY Virtual Machine**  
  Focus on:
  - The TOY simulator written in Java (TOY as a program that emulates TOY hardware)
  - How the simulator keeps arrays for registers, memory, and a program counter
  - The difference between:
    - **Simulation** (running one machine *inside* another by mimicking its instructions)
    - **Translation** (converting code in one language to another)
  - The Java Virtual Machine (JVM) as a more complex real-world virtual machine
  - The idea of **bootstrapping**: using one machine or language to define and build more powerful machines / environments

### Companion Website

Start from the chapter hub (same as last week):

- Chapter hub: <https://introcs.cs.princeton.edu/java/60machine/>

Then drill into:

- **6.3 TOY Programming**  
  <https://introcs.cs.princeton.edu/java/63programming/>

- **6.4 TOY Virtual Machine**  
  <https://introcs.cs.princeton.edu/java/64simulator/>

For each of 6.3 and 6.4:

- Read the main text
- Scroll to the bottom and at least skim:
  - **Q + A** for subtle points and clarifications
  - **Exercises** for small, concrete TOY problems
- Look at any downloadable `.toy` programs or sample input files; keep a few handy in your repo

---

## 2. No Quiz This Week

There is **no Week 9 quiz**.

Use that time to:

- Revisit earlier quiz questions that felt shaky  
- Re-work any conversions, two’s complement, or TOY questions that you got wrong the first time  
- Practice stepping through TOY code and predicting what PC, registers, and memory will look like after a few steps  

---

## 3. Final Exam Prep Guide

The Final is basically a “greatest hits” compilation of this quarter’s quizzes.

### 3.1 What the Final Will Look Like

- The Final Exam pulls from **quiz questions you have already seen**:
  - Expect familiar multiple choice questions (possibly re-ordered or with small tweaks)
  - Expect programming / reasoning problems in the same style as prior work
- You get **one attempt only** on the Final (no retakes like the quizzes)
- The Final is **asynchronous during Finals Week**:
  - Available all Finals Week
  - You choose when to start it
  - Once you start, the clock and one-shot attempt rules apply

### 3.2 Using Quizzes as a Study Bank

Because your instructor keeps the **highest grade per quiz**:

- You can **safely re-take** quizzes:
  - Figure out missed multiple-choice questions
  - Confirm which answers are correct  
  - Your score only goes **up or stays the same**, never down
- Strategy:
  1. Open your quiz attempts and identify questions you missed
  2. Re-work them using:
     - The textbook sections
     - Your weekly READMEs and notes
     - The companion website examples
  3. Re-take the quiz once you are confident, just to lock in the mental model

### 3.3 Screen Recording Requirements

For the Final Exam:

- You will **record your screen** to demonstrate you are not using unauthorized resources  
- Programming questions can be more time consuming, so:
  - Your instructor allows **separate recordings** for each programming task
  - That means you do not have to make one huge, single recording
- Before Finals Week:
  - Make sure you know how to:
    - Start and stop your chosen screen recording tool
    - Save and access the recordings
    - Keep only allowed windows / resources open while recording

---

## 4. TOY Practice Ideas For This Week

Here are some concrete things you can do while you read 6.3–6.4:

### 4.1 Hand-tracing TOY Programs

Pick a small TOY program from 6.3 or the companion site and:

1. Write down:
   - PC
   - Contents of each register
   - Any relevant memory locations
2. Step through one instruction at a time:
   - Update PC according to the instruction (increment or branch)
   - Update any register(s) or memory locations affected
3. Keep a short “execution trace”:
   - A table with rows like: step number, PC, instruction, changed registers, changed memory

Key skills:

- Reading hex opcodes and decoding opcode + operands in your head  
- Seeing how branches implement loops and conditionals  

### 4.2 Run the Java TOY Simulator

On the 6.4 page, look at `TOY.java` (the simulator) and:

- Notice how it:
  - Stores registers in an `int[]`
  - Stores memory in an `int[]`
  - Uses a single `int pc` to track the program counter
- If you get ambitious:
  - Download `TOY.java` and compile it
  - Run one of the sample `.toy` programs
  - Compare the simulator’s behavior to your hand-traced expectations

You do not have to deeply understand the entire implementation code, but it is helpful to see how a virtual machine is “just” a normal Java program with a loop.

### 4.3 Connect TOY to the JVM Conceptually

As you read, keep these parallels in the back of your mind:

- **TOY machine**  
  - Simple ISA, small instruction set, 16 registers, 256 memory words
  - Physical or imagined hardware, plus a simulator

- **TOY virtual machine (simulator)**  
  - Java code that treats TOY registers and memory as arrays
  - A big loop that:
    - Fetches the next instruction
    - Decodes opcode / operands
    - Executes behavior

- **Java Virtual Machine (JVM)**  
  - Much larger instruction set, designed for running compiled Java bytecode  
  - Still the same core idea: a virtual computer that you simulate on real hardware  

If you understand TOY plus TOY’s simulator, you have the mental model you need for the JVM.

---

## 5. Repo Notes (for Future Me)

Suggested structure for the Week 9 repo:

- `toy-programs/`  
  - `sum.toy` - example that sums a few numbers  
  - `max.toy` - finds the maximum of a small list in memory  
  - `countdown.toy` - simple loop and branch example  

- `src/`  
  - `ToyNotes.java` - a Java file where you jot down comments / experiments while reading `TOY.java`  
  - `ToyTraceHelper.java` (optional) - maybe a helper that prints out traces or decodes instructions in a more human readable way  

- `notes/`  
  - `week9-notes.md` - narrative notes on:
    - How TOY instructions map to higher-level patterns (if, while, for)
    - How TOY simulator is structured
    - How TOY compares to the JVM

- `exam-prep/`  
  - `quiz-review.md` - list of quizzes and topics to revisit  
  - Screenshots or text of any questions you initially missed and later fixed

The goal is that Future You can open Week 9 and immediately see:  
“Ah, this is where we actually programmed TOY and connected it to virtual machines, and this is where Final prep started in earnest.”
