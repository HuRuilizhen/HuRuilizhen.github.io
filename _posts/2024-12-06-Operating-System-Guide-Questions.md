---
layout: post
title: "Operating System Guide Questions"
description: " A list of guide questions (high level overview) to prepare for the OS exam."
date: 2024-12-06
feature_image: images/operating-system.jpg
tags: ['operating-system']
---

This post contains a list of high level overview questions to help prepare for the OS exam.

<!--more-->

## Table of Contents

- [Questions 🙋](#questions-)
  - [Question 1: What is a OS? What are the four major tasks performed by OS?](#question-1-what-is-a-os-what-are-the-four-major-tasks-performed-by-os)
  - [Question 2: How could a system be designed to allow a choice of operating system from which to boot? What would the bootstrap program need to do?](#question-2-how-could-a-system-be-designed-to-allow-a-choice-of-operating-system-from-which-to-boot-what-would-the-bootstrap-program-need-to-do)
  - [Question 3: What is the main advantage of the microkernel approach to system design? How do user programs and system services interact in a microkernel architecture? What are the disadvantages of using the microkernel approach?](#question-3-what-is-the-main-advantage-of-the-microkernel-approach-to-system-design-how-do-user-programs-and-system-services-interact-in-a-microkernel-architecture-what-are-the-disadvantages-of-using-the-microkernel-approach)
  - [Question 4: Why does an operating system design, in general, use layered approach? Please state its advantages and disadvantages.](#question-4-why-does-an-operating-system-design-in-general-use-layered-approach-please-state-its-advantages-and-disadvantages)
  - [Question 5: What is a process? What is a thread? Please compare them.](#question-5-what-is-a-process-what-is-a-thread-please-compare-them)
  - [Question 6: What resources are used when a thread is created?](#question-6-what-resources-are-used-when-a-thread-is-created)
  - [Question 7: What are the five states of a process in a computer system? Please describe the relationships of these five process states.](#question-7-what-are-the-five-states-of-a-process-in-a-computer-system-please-describe-the-relationships-of-these-five-process-states)
  - [Question 8: Describe the actions taken by a kernel to context-switch between processes.](#question-8-describe-the-actions-taken-by-a-kernel-to-context-switch-between-processes)
  - [Question 9: Why does an operating system have both the user mode and the kernel mode for processes? What are the advantages and disadvantages of the design?](#question-9-why-does-an-operating-system-have-both-the-user-mode-and-the-kernel-mode-for-processes-what-are-the-advantages-and-disadvantages-of-the-design)
  - [Question 10: Please list and describe three major activities of the operating system with regard to memory management, and three major activities of the operating system with regard to secondary storage management.](#question-10-please-list-and-describe-three-major-activities-of-the-operating-system-with-regard-to-memory-management-and-three-major-activities-of-the-operating-system-with-regard-to-secondary-storage-management)

---

# Questions 🙋

## Question 1: What is a OS? What are the four major tasks performed by OS?

An Operating System (OS) is a system software that manages **computer hardware** and **software resources** and provides common services for computer programs. It acts as an intermediary between users and the computer hardware. Without an OS, interacting with a computer would be much more complex, as all instructions would have to be given directly to hardware in a language the computer understands. The OS simplifies this interaction, making computers accessible to non-experts.

The four major tasks performed by an operating system include:

1. **Process Management**: The OS handles the creation and termination of both user and system processes. It also oversees suspending and resuming processes, blocking or allowing their execution, and allocating necessary resources to these processes. This ensures multiple programs can run concurrently without interfering with each other.
2. **Memory Management**: The OS manages the primary storage, RAM (Random Access Memory), ensuring that each process has enough memory space to operate and preventing conflicts between different processes. When physical memory is insufficient, the OS may use disk space as virtual memory.
3. **File System Management**: The OS provides a structured method for storing files so users can easily save, find, retrieve, and update files. It manages how files are organized, stored, retrieved, updated, and what permissions they have for access. For more practical details, refer to [xv6](https://github.com/mit-pdos/xv6-public), [xv6 docs](https://pdos.csail.mit.edu/6.828/2024/) and labs of [mmap](https://pdos.csail.mit.edu/6.828/2020/labs/mmap.html), [bigfile management](https://pdos.csail.mit.edu/6.828/2020/labs/fs.html).
4. **Device Management**: The OS controls and coordinates the use of various external devices (such as printers, scanners, etc.), ensuring they respond correctly to user requests. It communicates with hardware through device drivers, enabling hardware devices to be effectively used by applications.

## Question 2: How could a system be designed to allow a choice of operating system from which to boot? What would the bootstrap program need to do?

Normally, the bootstrap program loads the operating system at boot time. One design in use is to **make the bootstrap program load a separate program other than an OS kernel at boot time**. This separate program is called a **boot manager**. The boot manager knows the names and locations of the different OSes installed on the computer, and can load them into memory and execute them as required. The boot manager is configurable, either at boot time or by a separate user-level program that can be run before a reboot. Linux uses a boot manager known as GrUB (Grand Unified Bootloader), so Linux users can boot into either Linux or Windows if they choose. The bootstrap program itself resides in hardware and thus cannot be changed. Because the bootstrap program is fixed, it always loads a program from a fixed location in secondary storage. Either the OS (in the case of a machine that has as single OS installed) or the boot manager (in case of a machine that offers a choice of OSes) resides at this fixed location.

> Disclaimer: The answer to this question was obtained from [Quizlet OS Chapter 2 Flash Cards](https://quizlet.com/565216107/os-chapter-2-flash-cards/).

## Question 3: What is the main advantage of the microkernel approach to system design? How do user programs and system services interact in a microkernel architecture? What are the disadvantages of using the microkernel approach?

The main advantage of the microkernel approach to system design is its modularity and high reliability. In a microkernel architecture, only essential services for basic operation are implemented in the kernel, while all other services operate as separate processes in user space. This separation leads to several benefits:

- **Modularity**: Services can be added, removed, or updated independently without affecting the core system.
- **High Reliability**: If a service fails, it does not necessarily cause the entire system to crash, as failures are contained within the specific service.

**Interaction between User Programs and System Services:**
In a microkernel architecture, user programs interact with system services primarily through message-passing mechanisms. When a user program needs to access a service, it sends a message to the appropriate service process. The message contains the type of request and any necessary parameters. The service process receives the message, processes the request, and responds by sending a message back to the user program. This method enhances flexibility but introduces overhead due to crossing the boundary between user and kernel space.

**Disadvantages of Using the Microkernel Approach:**
While offering significant advantages, the microkernel approach also has drawbacks:

- **Performance Loss**: Frequent message passing and context switching can lead to lower performance compared to monolithic kernels, as each inter-process communication involves transitions from user to kernel space and back.
- **Increased Complexity**: Ensuring correct message passing and synchronization among different services adds complexity to system design. Development and debugging become more challenging as errors can occur across multiple separated services rather than being centralized in one kernel.
- **Resource Consumption**: Each service running as an independent process consumes certain resources, potentially leading to higher resource usage overall.

Other approaches, such as the multithreaded kernel, can also be used to achieve similar functionality, but with different trade-offs:
- **Monolithic Kernel:**
  - A monolithic kernel is the traditional design approach for an operating system kernel, where all operating system services—such as device drivers, file system management, network protocol stacks—are included and run within the kernel space. The advantage of this design is high efficiency because all operations are executed in kernel mode, reducing the overhead associated with switching between user mode and kernel mode.
  - However, its disadvantages are also evident: since all services operate within the same address space, if one service crashes or has a flaw, it can affect the stability and security of the entire system.

- **Hybrid Kernel:**
  - The hybrid kernel attempts to combine the benefits of both monolithic kernels and microkernels to achieve a balance between performance and modularity. In this architecture, certain core services remain in kernel space, while other services are moved into user space to run as separate processes.
  - Such a design increases system stability (since not all services run in kernel space) while retaining a degree of performance benefits. Windows NT, macOS, and some versions of Linux utilize a hybrid kernel design.

- **Exokernel:**
  - An exokernel represents a more radical design philosophy that provides only a minimal hardware abstraction layer, performing almost no resource management or protection itself but delegating these responsibilities to library operating systems (LibOS) at the application level. This means applications can directly control hardware resources, potentially offering higher performance and flexibility.
  - Despite providing high flexibility and potential performance gains, the exokernel also increases programming complexity, as developers must handle many low-level details themselves, which can be quite challenging for non-experts.

## Question 4: Why does an operating system design, in general, use layered approach? Please state its advantages and disadvantages.

Operating system design often employs a **layered approach** as a structured strategy to organize its functionalities and services into multiple layers. Each layer builds upon the one below it, providing services to the layer above. This architecture allows each layer to focus on specific sets of functions, simplifying the design, implementation, and maintenance of the system.

| Advantage                                                                | Disadvantage                                                           |
| ------------------------------------------------------------------------ | ---------------------------------------------------------------------- |
| Simplifies debugging and system verification by isolating layers.        | Can introduce additional overhead due to layer-to-layer communication. |
| Enhances modularity, making it easier to update or replace components.   | May lead to reduced performance due to abstraction layers.             |
| Allows for clear separation of responsibilities among system components. | Complexity in precise definition and enforcement of layer boundaries.  |

Also see a blog on AspiringYouths that discusses the layered approach for general system design: [AspiringYouths - Advantages and Disadvantages of Layered Approach To System Design](https://aspiringyouths.com/advantages-disadvantages/layered-approach-to-system-design/)

## Question 5: What is a process? What is a thread? Please compare them.

- A **process** is the fundamental unit of resource allocation and scheduling in an operating system. It represents an instance of a running program, with its own independent address space, memory, data stack, and other auxiliary data. Processes are isolated from each other; the crash of one process does not directly impact others.
- A **thread** is an execution unit within a process, sometimes referred to as a lightweight process. A single process can contain multiple threads, which share the same resources such as memory and file descriptors. Unlike processes, switching between threads has lower overhead because they share most of their context information.


| Characteristic        | Process                                                                                                             | Thread                                                                                                           |
| --------------------- | ------------------------------------------------------------------------------------------------------------------- | ---------------------------------------------------------------------------------------------------------------- |
| **Definition**        | An instance of a running program.                                                                                   | An execution path within a process.                                                                              |
| **Resources**         | Has its own separate memory space, file descriptors, etc.                                                           | Shares the resources (e.g., memory) of the parent process, but has its own stack, registers, etc.                |
| **Isolation**         | Highly isolated; a crash in one process typically does not affect others.                                           | Less isolated within the same process; a thread crash can cause the entire process to crash.                     |
| **Communication**     | Inter-Process Communication (IPC) is more complex, requiring special mechanisms (pipes, message queues, etc.).      | Intra-process communication is simpler, as threads can directly access shared data.                              |
| **Creation Overhead** | Creating a process is more expensive, as the OS needs to allocate new address spaces and other resources.           | Creating a thread is less expensive, as it does not need to duplicate most of the parent process's resources.    |
| **Context Switching** | Context switching costs are higher, involving saving and restoring more state information.                          | Context switching costs are lower, as only a smaller amount of state information needs to be saved and restored. |
| **Scheduling**        | Scheduled by the operating system scheduler.                                                                        | Can be scheduled by either the operating system or user-level libraries.                                         |
| **Multitasking**      | On a single-core CPU, processes appear to run concurrently over time. On multi-core CPUs, they can run in parallel. | More suitable for lightweight parallel or concurrent tasks within the same application.                          |
| **Usage**             | Suitable for running completely independent applications.                                                           | Suitable for task decomposition within the same application to improve responsiveness and resource utilization.  |

Also see my blogs on [Ray - Threads](https://huruilizhen.github.io/Threads) and [Ray - Processes](https://huruilizhen.github.io/Processes)

## Question 6: What resources are used when a thread is created? 

When a thread is created, the operating system allocates certain resources to ensure the new thread can operate correctly. Although threads share most of the resources of their parent process (such as address space, open files, and signal handling settings), each thread still requires some independent resources. Specifically:

- **Stack Space**:
    Each thread has its own stack for storing function calls, local variables, and return addresses. The size of the stack can be configured based on application needs and typically defaults to a relatively small fixed value.
- **Register Set**:
    Each thread has its own set of registers, including the program counter (PC), instruction register (IR), and other general-purpose registers. These registers hold key information about the thread's execution state.
- **Thread Control Block (TCB)**:
    The TCB is a data structure used by the OS to manage threads. It contains the thread identifier, scheduling information (priority, time slice), state (ready, running, waiting), and pointers to other relevant data structures.
- **Memory Allocation**:
    While threads share the address space of the process, additional memory allocation may be required in some cases, such as for thread-private data structures or dynamically allocated objects.
- **Synchronization Objects**:
    To coordinate work among multiple threads, synchronization mechanisms like mutexes, condition variables, or semaphores might be created to ensure threads safely access shared resources.
- **Other Resources**:
    Depending on the specific functionality of the thread, other types of resources might be involved, such as network connections, database handles, etc.

In summary, while threads are lighter than processes, creating a thread still consumes certain system resources, primarily stack space, register sets, thread control blocks, and related synchronization objects.

Also see my blog on [Ray - Threads](https://huruilizhen.github.io/Threads)

## Question 7: What are the five states of a process in a computer system? Please describe the relationships of these five process states.

In a computer system, a process can be in one of the following five states during its lifecycle:

- **New**:
    When a new process is created, it enters the New state. At this stage, the operating system allocates necessary resources for the process and records its information in the process table, but the process is not yet ready to execute.
- **Ready**:
    Once initialization is complete and all required resources are ready, the process transitions to the Ready state. Here, the process is prepared to run but awaits CPU time to begin execution. Processes in the Ready state are placed in one or more queues, awaiting selection by the scheduler.
- **Running**:
    When the scheduler selects a Ready process and assigns it to the CPU for execution, it moves into the Running state. This is the only state where the process actually executes instructions. On a single-core processor, only one process can be in the Running state at any given moment, whereas multi-core processors allow multiple processes to run concurrently.
- **Blocked (or Waiting)**:
    If a running process needs to wait for an event to occur (such as I/O completion, user input, semaphore, etc.), it transitions from the Running state to the Blocked state. In this state, the process cannot proceed until the awaited event occurs. For example, after initiating a disk read/write request, the process might enter the Blocked state until data transfer is complete.
- **Terminated (or Exit)**:
    When a process completes its tasks or encounters an unrecoverable error and ends, it enters the Terminated state. In this state, the operating system reclaims all resources used by the process and clears all associated information. Terminated processes no longer participate in system scheduling and execution.

Transitions between these states occur in a controlled and deterministic manner, ensuring that processes are managed efficiently and that all resources are properly allocated and released.

- **New to Ready**: After creation and initialization, a process moves from the New state to the Ready state.
- **Ready to Running**: When the scheduler selects a Ready process and assigns it CPU time, the process transitions from the Ready state to the Running state.
- **Running to Blocked**: If a running process needs to wait for some external event (like I/O), it changes from the Running state to the Blocked state until the event is completed.
- **Blocked to Ready**: Once the reason for blocking is resolved (such as I/O completion), the process returns to the Ready state, waiting for its next scheduling opportunity.
- **Running to Terminated**: Upon completing its work or encountering an unrecoverable error, a process directly enters the Terminated state, where the OS recovers its resources.
- **Ready to Terminated**: Although less common, under certain special circumstances, such as system shutdown or forced termination, a process can transition directly from the Ready state to the Terminated state.

{% include image_caption.html imageurl="/images/process-state.png" title="Process State" caption="Process State" %}

Also see my blog on [Ray - Processes](https://huruilizhen.github.io/Processes)

## Question 8: Describe the actions taken by a kernel to context-switch between processes.

When an operating system needs to switch from one process to another, it must save the state of the current process (its context) so that it can resume execution later and load the state of the next process to execute. This procedure is known as a context switch. Below are the typical steps taken by the kernel during a context switch:

1. **Save the Current Process's Context**:
    The kernel first saves all state information of the currently running process, including the contents of CPU registers (such as the program counter, instruction register, general-purpose registers, etc.), memory management information (page tables), and any other relevant data. This information is usually saved in the process's Process Control Block (PCB).
1. **Update Scheduling Information**:
    The kernel updates the scheduling information for the current process, such as changing its state from "Running" to "Ready" or "Blocked" and adjusting its priority or other scheduling parameters as needed.
2. **Select the Next Process**:
    The scheduler chooses the next process to execute based on certain algorithms (like First-Come-First-Served, Shortest Job Next, Round Robin, etc.). This may involve evaluating the priority, waiting time, and other factors of ready processes.
3. **Load the New Process's Context**:
    Once the new process is selected, the kernel loads its context, restoring all previously saved state information from its PCB back into the CPU registers and other relevant locations. This includes setting the program counter to point to the start address of the new process code and reconfiguring the Memory Management Unit (MMU) to reflect the new process's address space.
4. **Begin Execution of the New Process**:
    Finally, the kernel hands control over to the new process, allowing it to continue executing as if it had never been interrupted.
5. **Handle Synchronization and Resource Management**:
    In some cases, a context switch also involves handling synchronization issues (such as locks, semaphores) and resource management (such as file descriptors, network connections) to ensure the new process can safely access required resources without conflicting with the old process.

By following these steps, the kernel achieves smooth switching between processes, ensuring efficient operation of multitasking systems. Each context switch introduces some overhead, so optimizing scheduling strategies and minimizing unnecessary switches are crucial for enhancing system performance.

This structured explanation outlines the key actions taken by the kernel during a context switch, emphasizing the importance of preserving and restoring process states efficiently.

Also see my blog on [Ray - Processes](https://huruilizhen.github.io/Processes)

## Question 9: Why does an operating system have both the user mode and the kernel mode for processes? What are the advantages and disadvantages of the design?

Operating systems introduce User Mode and Kernel Mode to provide enhanced security and stability while efficiently managing hardware resources. These two modes define different levels of privileges for processes during execution:

- **User Mode**:
    Programs running in user mode do not have direct access to hardware or system resources. They can only request privileged operations through system calls to the operating system. This ensures that regular applications cannot directly modify critical system data or hardware states, thereby enhancing system stability and security.
- **Kernel Mode**:
    Code executing in kernel mode has the highest level of privilege, allowing direct access to all hardware resources and memory regions. The core parts of the operating system and drivers typically run in this mode. Due to their high privileges, these components must be carefully written to prevent errors from causing system-wide crashes.

**Advantages**

- **Enhanced Security**:
    User mode restricts direct access by applications to hardware and core system functionalities, reducing the likelihood of damage caused by malware or programming errors.
- **Improved Stability**:
    By separating non-privileged tasks from privileged ones, even if a program in user mode fails, it does not directly impact the kernel or other critical system components, thus improving overall system stability.
- **Simplified Debugging and Support**:
    Errors are isolated within specific modes, making issues easier to locate and fix. For instance, if a user program crashes, it won't affect other user programs or the kernel itself.

**Disadvantages**

- **Performance Overhead**:
    Each transition from user mode to kernel mode for a system call introduces additional overhead, including saving the current context, switching modes, and restoring the context, which can reduce system responsiveness.
- **Increased Complexity**:
    Designing and implementing two distinct privilege levels adds complexity to the system. Developers need to understand and correctly handle transitions between modes, increasing programming difficulty.
- **Potential Bottlenecks**:
    Frequent switches between the two modes, especially for applications requiring rapid responses, can become performance bottlenecks. Additionally, poor design can lead to deadlocks or other synchronization issues.

In summary, the design of user mode and kernel mode is a crucial feature in operating system architecture, offering higher levels of security and stability at the cost of some performance challenges and increased complexity. Proper configuration of these modes can maximize their benefits while minimizing adverse effects.

Also see my blog on [Ray - Processes](https://huruilizhen.github.io/Processes)

## Question 10: Please list and describe three major activities of the operating system with regard to memory management, and three major activities of the operating system with regard to secondary storage management.

**Three Major Activities in Memory Management**

- **Memory Allocation and Deallocation**:
    The operating system is responsible for allocating required memory space to processes and reclaiming it when the process ends or no longer needs these resources. This involves dynamically allocating and releasing memory blocks to ensure efficient use of limited physical memory resources. Various algorithms (such as Best Fit, First Fit) are used to decide how to allocate free memory blocks to requesting processes.
- **Address Translation**:
    To protect each process's independence and simplify programming models, the OS implements virtual memory mechanisms. It uses hardware support (like MMU, Memory Management Unit) to translate program-used virtual addresses into actual physical addresses. This translation not only enhances security in multitasking but also allows programs to access a larger address space than the actual physical memory, known as virtual memory.
- **Page Replacement and Swapping**:
    When available physical memory is insufficient, the OS must decide which pages should be moved out of memory to make room for new or active pages. Page replacement algorithms (such as LRU, Least Recently Used) are used to select suitable pages for replacement. Additionally, entire processes can be swapped out to a swap area on disk (Swapping), making more memory available for higher-priority processes.

**Three Major Activities in Secondary Storage Management**

- **File System Management**:
    The OS provides an abstract interface for managing file and directory structures. This includes creating, deleting, reading, writing files, as well as renaming and moving files and directories. The file system also maintains metadata about files (such as permissions, timestamps) and ensures data consistency and integrity.
- **Storage Allocation and Deallocation**:
    Similar to memory management, the OS manages the allocation and reclamation of storage space on disk. It handles partitioning, volume, and file allocation strategies on disk to ensure efficient use of disk space. When files are deleted or updated, the OS adjusts the storage layout on disk accordingly to avoid fragmentation issues.
- **Data Protection and Recovery**:
    The OS employs various measures to protect data stored in secondary storage from hardware failures, software errors, or other anomalies. Examples include implementing backup mechanisms, logging, snapshot features, and redundant storage technologies (such as RAID). Moreover, the OS must provide effective recovery tools and services to quickly restore data in case of data loss or corruption.

These structured descriptions outline key activities in both memory and secondary storage management, highlighting the essential roles played by the operating system in ensuring efficient and secure resource utilization.

---

Related Posts / Websites 👇

📑 [Quizlet - OS Final Flash Cards](https://quizlet.com/465647132/os-final-flash-cards/)

📑 [Ray - Threads](https://huruilizhen.github.io/Threads)

📑 [Ray - Processes](https://huruilizhen.github.io/Processes)