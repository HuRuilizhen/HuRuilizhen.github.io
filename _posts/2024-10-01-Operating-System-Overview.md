---
layout: post
title: "Operating System Overview"
description: "High-level overview of Operating System"
date: 2024-10-01
feature_image: images/operating-system.jpg
tags: ['operating-system']
---

An operating system (OS) is a software that manages the hardware and software resources of a computer. It provides the interface between the users of a computer and that computer’s hardware. 

<!--more-->

---

# Model of Computer

An operating system has to deal with the fact that a computer is made up of

- **CPU (Central Processing Unit)**: 

    The CPU is a component of the computer that executes the instructions. It is responsible for the flow of data and instructions between the input and output devices.

- **I/O (Input/Output)**: 

    The I/O is a component of the computer that provides input and output. It is used to read and write data to the computer.

- **RAM (Random Access Memory)**: 

    RAM is a type of volatile memory that is used to store data and machine code currently being used by the CPU. It allows for fast read and write access to a storage medium that is directly accessible by the CPU. The data in RAM is lost when the computer is turned off, which is why it is used for temporary storage. RAM plays a crucial role in determining the performance of a system, as it affects the speed at which programs run and how many programs can be run simultaneously.

    - **Volatile Memory**: Data is lost when power is off.
    - **Fast Access**: Provides quick read/write speeds.
    - **Temporary Storage**: Holds data for currently running processes and tasks.

- **Disk Storage**: 

    Disk storage refers to non-volatile storage that retains data even when powered off. This includes hard disk drives (HDDs) and solid-state drives (SSDs). Unlike RAM, disk storage is slower but provides larger storage capacity for permanent data storage. Disk storage is used to store the operating system, applications, and user data. The speed of disk storage affects how quickly data can be loaded into RAM and hence can impact system performance during data-intensive operations.

    - **Non-Volatile Memory**: Data remains intact without power.
    - **Larger Capacity**: Offers more storage space compared to RAM.
    - **Permanent Storage**: Used for long-term data retention like files and applications.
  
---

# OS Concepts

An operating system (OS) provides the interface between the users of a computer and that computer’s hardware.

- Manages the ways applications access the resources in a computer, including its `disk drives`, `CPU`, `main memory`, `input devices`, `output devices`, and `network interface`.
- Manages the multiple users of the computer.
- Manages the multiple applications running at the same time.

**Layered Model** is a hierarchical model of an operating system, with the following layers:

- **Hardware Layer (Bottom Layer)**: Provides the basic computing, storage, and I/O capabilities.
- **Kernel Layer (Middle Layer)**: Core of the OS, including process management, memory management, file system, device drivers, and network communication.  It sits between the hardware and the applications, providing essential services and enhancing security and stability.
- **User Interface and Application Layer (Top Layer)**: Interacts with the hardware indirectly by calling kernel-provided services, allowing users and applications to perform tasks without needing to know the underlying hardware details.

User applications don’t communicate directly with low-level hardware components, and instead delegate such tasks to the kernel via **System Calls**. Key features include:

- **Interface Functionality**: Provides a standardized interface for applications to communicate with the kernel.
- **Privilege Level**: The only legitimate way for user-mode programs to enter kernel mode and execute privileged operations.
- **Performance Consideration**: Mode switching incurs overhead, so system calls should be used judiciously to avoid performance degradation.
- **Security**: Ensures that only validated operations are executed, enhancing the security and stability of the system.

**System Calls** example:

- File Operations: `open`, `close`, `read`, `write`, etc.
- Process Management: `fork`, `execve`, `wait`, `kill`, etc.
- Memory Management: `malloc`, `free`, `mmap`, `munmap`, etc.
- Socket Operations: `connect`, `accept`, `recv`, `send`, etc.

## Process

A process is a running instance of an application. It is a collection of instructions that are executed by the operating system.

- The actual contents of all programs are initially stored in persistent storage, such as a hard drive.
- In order to be executed, a program must be loaded into RAM and uniquely identified as a process. Hence, multiple copies of the same program can be run as different processes.
- It is identified by a unique nonnegative integer, called the process ID (PID). Users associate its CPU time, memory usage, user ID (UID), program name, etc. with the process ID.

## File System

An abstraction of how the external, nonvolatile memory of the computer is organized.

- organizefiles hierarchically into **folders**, also called **directories**. 
- Each folder may contain files and/or subfolders
- Consists of a collection of nested `directories` and `files` forms a **tree**
- The topmost folder is the **root** of this tree and is also called the root folder.
  
**File permissions** are checked by the operating system to determine if a file is readable, writable, or executable by a user or group of users.

In Unix-like OS’s, a file permission matrix shows who is allowed to do what to the file.

{% include image_caption.html imageurl="/images/file-system-permission-example.png" title="File System Permissions in MacOS" caption="File System Permissions in MacOS" %}

## Memory Management

The RAM of computer is its **address space**. It contains the program code, data and working memory. For any running process, it is organized into different segments. These segments are called **pages**.

**Memory Organization** contains:
- **Text**: Instructions that are executed by the CPU.
- **Data**: Static program variables that have been initialized in the program code.
- **BBSS**: Contains static variables that are uninitialized.
- **Stack**: Used for keeping track of the call structure of subroutines. (e.g., function calls).
- **Heap**: Used for dynamically allocating memory during program execution.

| Page  |
| :---: |
| Stack |
|   ⬇   |
|   ⬆   |
| Heap  |
|  BSS  |
| Data  |
| Text  |

**Virtual Memory**: A memory management technique that gives an application the impression it has **contiguous working memory**, while in fact, it may be fragmented and spread across various physical memory locations. This abstraction simplifies programming by allowing processes to use more memory than physically available, as the OS handles the mapping to physical addresses. It also enhances system security and stability by isolating process memory.

{% include image_caption.html imageurl="https://www.cs.uic.edu/~jbell/CourseNotes/OperatingSystems/images/Chapter9/9_01_VirtualMemoryLarger.jpg" title="Virtual Memory" caption="Virtual Memory" %}

---

Related Posts 👇

📑 [Processes](https://huruilizhen.github.io/Processes) and [Threads](https://huruilizhen.github.io/Threads)

📑 [File System](https://huruilizhen.github.io/File-System)

📑 [Memory Management](https://huruilizhen.github.io/Memory-Management)