---
layout: post
title: "Processes"
description: "Introduction to Processes in Operating System"
date: 2024-10-02
feature_image: images/operating-system.jpg
tags: ['operating-system']
---

Processes are a fundamental concept in operating systems. They are used to manage resources and perform tasks. This blog will cover process concept, process scheduling, and process operations.

<!--more-->

---

# Process Concept 💡

An operating system executes a variety of programs that run as a process. We can say that **a process is a program in execution**. The process execution must progress in sequential fashion. <font color="red">No parallel execution of instructions of a  single process.</font> 

- Program is passive entity stored on disk (executable file) and process is active 
- Program becomes process when an executable file is loaded into memory
Execution of program started via GUI mouse clicks, command line entry of its name, etc.
- One program can be several processes: consider multiple users executing the same program

## Process Structure

- **Process ID (PID)**: This is a unique identifier assigned to each process. It is used to identify the process in the system.
- **Program Code/Text Segment**: This is the set of machine instructions that the program actually executes. It is read-only because there is no need to modify the compiled code.
- **Stack**: This is a Last-In-First-Out (LIFO) data structure used to store information during function calls, such as local variables, function parameters, and return addresses. Each time a function is called, a new stack frame is pushed onto the top of the stack; when the function returns, the corresponding stack frame is popped off.
- **Data Segment**: This part contains initialized global and static variables. These variables are allocated space before the program starts executing and persist throughout the program's lifetime.
- **Heap**: Unlike the data segment, the heap is a region of memory that is dynamically allocated during the program's execution as needed. Programmers use language-specific functions (such as `malloc` and `free` in C) to request and release this memory. The heap typically grows from lower to higher memory addresses.
- **Current Activity**: These information are stored in the process control block (PCB) of the process. See `Process Control Block` for more details.

{% include image_caption.html imageurl="/images/process-structure.png" title="Process Structure" caption="Process Structure" %}

## Process State

When a process executes, it is in one of the following states:

- **New**:  The process is being created
- **Running**:  Instructions are being executed
- **Waiting**:  The process is waiting for some event to occur
- **Ready**:  The process is waiting to be assigned to a processor
- **Terminated**:  The process has finished execution

{% include image_caption.html imageurl="/images/process-state.png" title="Process State" caption="Process State" %}

## Process Control Block (PCB)

Each process in an operating system is represented by a data structure known as the **Task Control Block (TCB)** or **Process Control Block (PCB)**. This block contains all the information necessary to manage and control the execution of a process. The key components include:

- **Process State**: The current state of the process.
- **Program Counter (PC)**: The address of the next instruction to be executed. This is crucial for resuming the process after it has been preempted or interrupted.
- **CPU Registers**: The contents of all registers that are specific to the process, including:
  - **General-purpose registers**: Used for arithmetic and logical operations.
  - **Status registers**: Store the status of the CPU, such as flags indicating overflow, zero, carry, etc.
  - **Stack pointer (SP)**: Points to the top of the stack.
  - **Base pointer (BP)**: Used to reference parameters and local variables in the stack frame.
- **CPU Scheduling Information**: Information used by the scheduler to determine when and how the process should be executed, including:
  - **Priority**: A value that determines the relative importance of the process.
  - **Scheduling queue pointers**: Pointers to the scheduling queues (e.g., ready queue, wait queue) where the process is placed.
- **Memory Management Information**: Details about the memory allocated to the process, including:
  - **Virtual memory space**: The total address space available to the process.
  - **Page tables**: Maps virtual addresses to physical addresses.
  - **Segmentation information**: If the memory is divided into segments, this includes the base and limit of each segment.
- **Accounting Information**: Data used for resource management and billing, including:
  - **CPU usage**: The amount of CPU time consumed by the process.
  - **Elapsed clock time**: The total time since the process started.
  - **Time limits**: Maximum allowed CPU time, wall-clock time, etc.
- **I/O Status Information**: Information about the I/O operations associated with the process, including:
  - **Allocated I/O devices**: Devices that are currently assigned to the process.
  - **List of open files**: Files that the process has opened and is using.

<table style="margin: 0 auto; border-collapse: collapse; text-align: center;">
  <tr>
    <th style="border: 1px solid black;">Process Control Block (PCB)</th>
  </tr>
  <tr>
    <td style="border: 1px solid black; word-break: break-word;">
      <ul>
        <li>Process ID</li>
        <li>Process State</li>
        <li>Program Counter</li>
        <li>CPU Registers</li>
        <li>CPU Scheduling Information</li>
        <li>Memory Management Information</li>
        <li>Accounting Information</li>
        <li>I/O Status Information</li>
      </ul>
    </td>
  </tr>
</table>

---

# Process Scheduling 🎛️

The process scheduler is responsible for selecting the next available process to execute on the CPU core. Its primary goals are:

- **Maximize CPU Utilization**: Ensure that the CPU is utilized as much as possible, reducing idle time.
- **Quickly Switch Processes**: Efficiently switch a process from the ready state to the running state to improve system responsiveness and throughput.

## Process Scheduling Mechanisms

To achieve these goals, the process scheduler maintains several different scheduling queues:

- **Ready Queue**:
   - **Definition**: The ready queue contains all processes that are ready and waiting to execute. These processes are already in main memory and have all the necessary conditions to start executing.
   - **Function**: When the CPU is idle or the currently running process is preempted, the scheduler selects a process from the ready queue to execute.
- **Wait Queues**:
   - **Definition**: Wait queues contain processes that are waiting for an event (such as I/O completion). These processes cannot execute temporarily because they depend on external events.
   - **Function**: Once the required event occurs, the process is moved to the ready queue, ready to execute again.

## Process Migration
  
Processes migrate among different queues during their lifecycle, as follows:

- **New -> Ready**: When a new process is created, it is placed in the ready queue, waiting for CPU resources.
- **Ready -> Running**: When the scheduler selects a process from the ready queue, the process is loaded onto the CPU and enters the running state.
- **Running -> Waiting**: If a running process needs to wait for an event (such as I/O), it is moved to a wait queue.
- **Waiting -> Ready**: Once the awaited event completes, the process is moved back to the ready queue, ready to execute again.
- **Running -> Terminated**: When a process completes its task or is terminated for some reason, it is removed from the system.

{% include image_caption.html imageurl="/images/process-migration.png" title="Process Migration" caption="Process Migration" %}

## Context Switch

When the scheduler decides to switch from one process to another, it needs to save the current process's state (including the program counter, registers, etc.) and then load the next process's state. This process is called context switching.

- **Process Context**: The context of a process is represented in the Process Control Block (PCB).
- **Context-Switch Time**: Context-switch time is pure overhead; the system does no useful work while switching.
- **Complexity of OS and PCB**: The more complex the operating system and the PCB, the longer the context switch.
- **Hardware Support**: Context-switch time depends on hardware support.
- **Multiple Sets of Registers**: Some hardware provides multiple sets of registers per CPU, allowing multiple contexts to be loaded at once.

{% include image_caption.html imageurl="/images/process-context-switch.png" title="Process Context Switch" caption="Process Context Switch" %}

---

# Operation on Processes 📝

System must provide mechanisms for `Process Creation` and `Process Termination`.

## Process Creation

- **Process Identifier (PID)**: Processes are typically identified and managed via a process identifier (PID).
- **Process Tree**: A parent process creates child processes, which in turn can create other processes, forming a tree of processes.
{% include image_caption.html imageurl="/images/process-tree.png" title="Process Tree" caption="Process Tree" %}
- **Resource Sharing Options**:
  - Parent and children share all resources.
  - Children share a subset of the parent's resources.
  - Parent and child share no resources.
- **Execution Options**:
  - Parent and children execute concurrently.
  - Parent waits until children terminate.
- **Address Space**:
  - Child is a duplicate of the parent.
  - Child has a new program loaded into it.
- **UNIX Examples**:
  - **`fork()` System Call**: The `fork()` system call creates a new process that is almost identical to the parent process. The new process (child process) inherits the memory space, open file descriptors, environment variables, etc., of the parent process.
  - **`exec()` Family of Functions**: The `exec()` family of functions is used to load and run a new program into the current process, replacing the existing program code. Common `exec()` functions include `execl()`, `execv()`, `execle()`, `execve()`, etc.
  - **`wait()` System Call**: The `wait()` system call causes the parent process to pause execution until one of its child processes terminates. The parent process can use `wait()` to retrieve the exit status information of the child process.
{% include image_caption.html imageurl="/images/process-examples.png" title="Process Examples" caption="Process Examples" %}

## Process Termination

- **Normal Termination**:
  - The process executes the last statement and then asks the operating system to delete it using the `exit()` system call.
  - Status data is returned from the child to the parent via the `wait()` system call.
  - The operating system deallocates the process's resources.

- **Forced Termination**:
  - The parent process may terminate the execution of child processes using the `abort()` system call. Some reasons for doing so include:
    - The child has exceeded allocated resources.
    - The task assigned to the child is no longer required.
    - The parent is exiting, and the operating system does not allow a child to continue if its parent terminates.
  - Some operating systems do not allow a child to exist if its parent has terminated. If a process terminates, then all its children must also be terminated.
  - **Cascading Termination**: All children, grandchildren, etc., are terminated.
  - The termination is initiated by the operating system.

- **Waiting for Child Process Termination**:
  - The parent process may wait for the termination of a child process by using the `wait()` system call. The call returns status information and the PID of the terminated process.
    ```c
    pid = wait(&status);
    ```
  - If no parent is waiting (i.e., did not invoke `wait()`), the process becomes a zombie.
  - If the parent terminates without invoking `wait()`, the process becomes an orphan.
- **Zombie Process**: When a child process terminates, if the parent process does not call `wait()` to retrieve its status information, the child process becomes a zombie. A zombie process has already terminated, but its PCB still exists in the system until the parent process calls `wait()` or the parent process itself terminates.
- **Orphan Process**: When the parent process terminates without calling `wait()`, the child process becomes an orphan. In this case, the operating system typically sets the parent of these orphan processes to the `init` process (PID 1) to ensure they are properly managed and cleaned up.

---

Related Posts 👇

📑 [Operating System Overview](/Operating-System-Overview)

📑 [Operating System Security](/Operating-System-Security)

📑 [Threads](/Threads)

📑 [File System](/File-System)

📑 [Memory Management](/Memory-Management)