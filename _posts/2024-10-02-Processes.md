---
layout: post
title: "Processes"
description: "Introduction to Processes in Operating System"
date: 2024-10-02
feature_image: images/operating-system.jpg
tags: ['Operating-System']
---

Processes are a fundamental concept in operating systems. They are used to manage resources and perform tasks.

<!--more-->

---

# Process Concept 

## Process Structure

- **Process ID (PID)**: This is a unique identifier assigned to each process. It is used to identify the process in the system.

- **Program Code/Text Segment**: This is the set of machine instructions that the program actually executes. It is read-only because there is no need to modify the compiled code.

- **Stack**: This is a Last-In-First-Out (LIFO) data structure used to store information during function calls, such as local variables, function parameters, and return addresses. Each time a function is called, a new stack frame is pushed onto the top of the stack; when the function returns, the corresponding stack frame is popped off.

- **Data Segment**: This part contains initialized global and static variables. These variables are allocated space before the program starts executing and persist throughout the program's lifetime.

- **Heap**: Unlike the data segment, the heap is a region of memory that is dynamically allocated during the program's execution as needed. Programmers use language-specific functions (such as `malloc` and `free` in C) to request and release this memory. The heap typically grows from lower to higher memory addresses.

- **Current Activity**:
  - **Program Counter (PC)**: It points to the location of the next instruction that the CPU will execute.
  - **Processor Registers**: These are small storage areas within the CPU used to hold operands and intermediate results. For example, general-purpose registers are used for arithmetic and logical operations, while status registers store the CPU's state flags.

{% include image_caption.html imageurl="/images/processes-structure.png" title="Process Structure" caption="Process Structure" %}
