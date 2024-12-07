---
layout: post
title: "Main Memory and Virtual Memory"
description: "High Level Discussion of Main Memory and Virtual Memory"
date: 2024-11-01
feature_image: images/operating-system.jpg
tags: ['operating-system']
---

This post provides a high level discussion of main memory and virtual memory. It serves as a summary of the main concepts and key points to know about main memory and virtual memory in the context of operating systems.

<!--more-->

---

Before we start discussing the main memory and virtual memory, let's first briefly introduce the concept of the main memory and virtual memory.

**Main Memory (Primary Storage):**
- Main memory, also known as RAM (Random Access Memory), is the primary working storage in a computer.
- It features **fast read and write speeds** and is used to store programs and data that are **currently in use**.
- RAM is **volatile**, meaning the information it holds is lost when the computer is turned off.
- Main memory **interacts directly** with the CPU, so its performance significantly impacts the overall efficiency of the system.
- The size of main memory is **limited** and is typically much smaller than a hard drive but has a much faster access speed.

**Virtual Memory:**
- Virtual memory is a memory management technique that enables the operating system to **use space on the hard disk to extend physical memory**.
- With virtual memory, each program can assume it has a **contiguous, isolated address space**, even if the actual physical memory is fragmented or insufficient.
- The virtual memory system divides memory into fixed-size blocks called **pages**. Similarly, the physical memory is divided into equally sized blocks called **page frames**.
- When there is not enough physical memory, inactive pages are swapped to a swap file or partition on the hard disk; this process is called **paging**.
- Virtual memory allows for running **larger** programs and **more** programs concurrently without actually increasing the capacity of physical memory.
- Using virtual memory may introduce some **overhead** because it requires managing page tables, handling page faults, and possibly performing disk I/O operations.

## Table of Contents

- [Main Memory](#main-memory)
  - [Background ️and Concepts](#background-️and-concepts)
- [Virtual Memory](#virtual-memory)
  - [Background ️and Concepts](#background-️and-concepts-1)
  - [Demand Paging](#demand-paging)
    - [Valid-Invalid Bit](#valid-invalid-bit)
    - [Steps in Handling Page Faults](#steps-in-handling-page-faults)
    - [Aspects of Demand Paging](#aspects-of-demand-paging)
    - [Stages in Demand Paging](#stages-in-demand-paging)
    - [Performance of Demand Paging](#performance-of-demand-paging)
  - [Copy-on-Write](#copy-on-write)
  - [Page Replacement](#page-replacement)
  - [Allocation of Frames](#allocation-of-frames)
  - [Thrashing](#thrashing)
  - [Memory-Mapped Files](#memory-mapped-files)
  - [Allocating Kernel Memory](#allocating-kernel-memory)

---

# Main Memory

## Background ️and Concepts

---

# Virtual Memory 

## Background ️and Concepts 

Code needs to be in memory to execute, but the entire program is rarely used all at once. Parts of a program such as error-handling code, unusual routines, or large data structures may not be accessed frequently. Therefore, it's not necessary to have the entire program code loaded into memory simultaneously. Considering this, there should be an ability to **execute partially-loaded programs, loading only the required portions**.

With virtual memory technology, programs are no longer constrained by the limits of physical memory. Each running program takes up less memory while executing, allowing **more programs to run concurrently**. This leads to **increased CPU utilization and throughput** without increasing response time or turnaround time. Less I/O is needed to load or swap programs into memory, which means **each user program runs faster**.

During discussion, we first define some terminology and then go into the details of virtual memory:

- **Page**:
    A fixed-size block of virtual memory, typically several kilobytes (e.g., 4KB), representing the smallest unit that can be independently loaded into memory.
- **Page Frame**:
    A fixed-size block within physical memory used to store pages mapped from the virtual address space. Page frames are units into which physical memory is organized.
- **Segment**:
    A logical part of a program with variable size. Each segment has its own starting address and length, representing a set of related instructions or data.
- **Demand Paging**:
    A memory management strategy where not all pages of a program are loaded into memory at once but only when needed. When a process attempts to access a page not in memory, a page fault occurs, and the OS loads the required page from disk into an available page frame.
- **Demand Segmentation**:
    A memory management strategy based on segments, where each segment represents a logical part of a program. Only the segments that are needed are loaded into memory. Unlike paging, segments do not have a fixed size but are determined by the actual needs of the program.

- **Page Table**:
    A data structure maintained by the operating system to manage the mapping from virtual addresses to physical addresses. It contains entries for each page in the virtual address space indicating which physical page frame it corresponds to.
- **Memory Management Unit (MMU)**:
    A hardware component responsible for translating logical addresses into physical addresses. It uses the page table for address translation and handles page faults.
- **Page Fault**:
    An exception that occurs when a process attempts to access a page not present in physical memory. The OS responds to this error by loading the necessary page from disk into memory.
- **Address Mapping Mechanism**
When a process attempts to access an address, the MMU looks up the corresponding physical address based on the current page table. If the page is not in memory (a page fault occurs), the operating system loads the necessary page from disk into memory and updates the page table.

- **Virtual Address Space**:
  - The virtual address space is the logical address range provided by the operating system for each process. Typically, the logical address space for the stack is designed to start at the maximum logical address and grow "downward," while the heap grows "upward." This design maximizes the use of the address space.
  - The unused address space between the stack and the heap forms a "hole," which allows for efficient memory usage. No physical memory is allocated until the heap or stack grows into a new page. This approach enables sparse address spaces with holes left for future growth, dynamically linked libraries, etc.
  - System libraries can be shared via mapping into the virtual address space, allowing multiple processes to share a single copy of the library, thus saving memory. Shared memory can be achieved by mapping pages read-write into the virtual address space. Pages can also be shared during the fork() call, speeding up process creation.

{% include image_caption.html imageurl="/images/virtual-address-space.png" title="Virtual Address Space" caption="Virtual Address Space"%}

## Demand Paging

- **Page Replacement with Prediction**:
    In traditional paging systems with swapping, the pager (the component responsible for page management) tries to predict which pages will be used in the future and makes decisions about which pages can be safely swapped out. This guessing is not always accurate and can lead to unnecessary I/O operations and memory wastage.
- **On-Demand Loading in Demand Paging**:
    Conversely, in demand paging systems, the OS does not pre-load pages but only brings them into memory when they are actually needed. This reduces unnecessary I/O operations, improves memory utilization, and speeds up process startup.

- **Determining the Set of Pages to Load**:
  - **Working Set Theory**: The OS tracks each process's recent access patterns to estimate its current working set.
  - **Page Fault Handling**: When a process attempts to access a page not in memory, a page fault occurs. The OS catches this fault and loads the required page from disk into memory.
  - **Page Replacement Algorithms**: Algorithms like LRU (Least Recently Used) and FIFO (First In First Out) help decide which pages should remain in memory and which can be removed.
  - **New MMU Functionality**: Modern Memory Management Units (MMUs) provide additional features to support demand paging, such as TLB caching and multi-level page tables, which enhance address translation speed and performance.

- **Behavior Based on Page Residency**:
  - **If the Needed Pages Are Already in Memory**: There is no difference in behavior between demand paging and non-demand paging systems. The process can directly access the required pages without additional actions.
  - **If the Needed Pages Are Not in Memory**: The OS must detect this situation and load the required page from storage (e.g., disk) into memory. This process is transparent, ensuring that program behavior remains unchanged and programmers do not need to modify their code.

- **Transparency**:
    A key characteristic of demand paging is its transparency. Programs run as if they have access to a complete and contiguous address space, without needing to know how memory is managed at the lower levels. The OS and hardware work together to ensure that page loading and unloading are completely transparent to applications, maintaining program consistency and stability.

Demand paging achieves more efficient, flexible memory management and higher system performance by loading pages on-demand rather than relying on predictive methods. It uses working set theory and page fault handling to intelligently determine which pages should be loaded into memory and leverages new MMU functionalities to optimize this process. Additionally, demand paging maintains transparency for applications, allowing developers to benefit from performance improvements without changing their code.

### Valid-Invalid Bit

Each page table entry is associated with a valid-invalid bit:
- **V (Valid)**: Indicates that the page is currently in memory (memory resident).
- **I (Invalid)**: Indicates that the page is not in memory.

Initially, the valid-invalid bit for all entries in the page table is set to I, assuming that no pages are in memory at startup.

For example, a snapshot of a page table might look like this:

| Page Number | Frame Number | Valid-Invalid Bit |
| ----------- | ------------ | ----------------- |
| 0           | 5            | V                 |
| 1           | -            | I                 |
| 2           | 7            | V                 |
| 3           | -            | I                 |

In this example, pages 0 and 2 have their valid-invalid bits set to V, indicating they are in physical memory, while pages 1 and 3 have their bits set to I, indicating they are not currently in memory.

### Steps in Handling Page Faults

When a program attempts to access a page, if that page is not in memory, it triggers a page fault. The operating system captures this fault and handles it through the following steps:

1. **First Reference to the Page**:
    When a process first references a page that is not in memory, it causes a page fault, and control is transferred to the operating system.
2. **Operating System Check**: The operating system checks the page table to decide how to proceed
   - **Invalid Reference**: If the page table entry is marked as invalid (I) due to an illegal address, the process is aborted.
   - **Just Not in Memory**: If the page is simply not in memory but the reference is valid, continue to the next step.
3. **Find a Free Frame**:
    The operating system needs to find a free frame to hold the incoming page. If all frames are occupied, it selects a page to replace using a page replacement algorithm.
4. **Swap Page into Frame**:
    The operating system schedules a disk operation to load the required page from disk or swap space into the found free frame.
5. **Update Page Table**:
    Update the page table entries to indicate that the page is now in memory. Specifically, set the valid-invalid bit to valid (V) and record the corresponding physical frame number.
6. **Set Validation Bit**:
    Set the validation bit to V, indicating that the page has been successfully loaded into memory.
7. **Restart the Instruction**:
    Return control to the process that caused the page fault, re-executing the instruction that led to the fault, ensuring the program can continue running normally.

Through these steps, the operating system ensures that only the pages actually needed are loaded into memory, improving memory utilization and reducing unnecessary I/O operations.

{% include image_caption.html imageurl="/images/handling-page-faults.png" title="Handling Page Faults" caption="handling page faults"%}

### Aspects of Demand Paging

- **Extreme Case: Start Process with No Pages in Memory**:
    In a demand paging system, an extreme case is starting a process with no pages in memory. The OS sets the instruction pointer to the first instruction of the process, which is non-memory-resident, leading to an immediate page fault. <u>For every other process page, the same situation occurs on the first access, triggering a page fault.</u>
- **Pure Demand Paging**:
    Pure demand paging means that all pages are loaded only when needed. In reality, a given instruction could access multiple pages, resulting in multiple page faults.
- **Pain Decreased Because of Locality of Reference**:
    The principle of locality of reference reduces the performance issues caused by frequent page faults. Programs tend to repeatedly access the same or adjacent pages over short periods, allowing the OS to manage memory more efficiently and reduce the number of page faults.
- **Hardware Support Needed for Demand Paging**:
  - Page Table with Valid / Invalid Bit: Each page table entry includes a valid-invalid bit to indicate whether the page is in memory.
  - Secondary Memory (Swap Device with Swap Space): The system requires a disk or another form of secondary storage to hold infrequently used pages, enabling them to be reloaded into memory when needed.
  - Instruction Restart Mechanism: When a page fault occurs, the OS must be able to <u>recover and re-execute the instruction that caused the page fault</u>, ensuring the program can continue running normally.
- **Free-Frame List**:
  - When a page fault occurs, the operating system must bring the desired page from secondary storage into main memory.
  - Most operating systems maintain a free-frame list, a pool of free frames for satisfying such requests.
  - Operating system typically allocate free frames using a technique known as zero-fill-on-demand, ensuring the content of the frames zeroed-out before being allocated.
  - When a system starts up, all available memory is placed on the free-frame list.

{% include image_caption.html imageurl="/images/free-frame-list.png" title="Free Frame List" caption="free frame list"%}

Through these mechanisms, a demand paging system can efficiently manage and allocate memory under limited resources, minimizing the impact on program performance.

### Stages in Demand Paging

In a demand paging system, when a page fault occurs, the operating system needs to execute a series of operations to handle this situation. Below are the detailed steps involved in handling a page fault, especially in the worst case:

1. **Trap to the Operating System**:
    When a process attempts to access a page not in memory, it triggers a page fault, transferring control to the operating system.
2. **Save User Registers and Process State**:
    The OS saves the current process's user registers and state information for later restoration.
3. **Determine That the Interrupt Was a Page Fault**:
    The OS checks and confirms that the interrupt was caused by a page fault.
4. **Check Legality of Page Reference and Locate Disk Position**:
   1. **Check Page Reference Legality**: Ensure the page reference is legal (not an illegal address).
   2.  **Determine Disk Location**: Identify the specific location of the page on disk or swap space.
5. **Issue a Read from Disk to a Free Frame**:
    The OS schedules a disk read operation to load the required page into a free frame.
6. **Wait for Device to Service Read Request**:
   1.  **Wait in Queue**: If the disk is busy with other requests, the current request must wait in a queue until the disk can service it.
   2. **Wait for Seek and Latency Time**: The disk completes its seek and rotational latency before beginning data transfer.
7. **Begin Transfer of Page to Free Frame**:
   1. **Start Transfer**: The disk begins transferring the page data to the specified free frame.
   2. **Allocate CPU to Other Users**: While waiting for the disk transfer, the OS can allocate the CPU to other processes to improve system utilization.
8. **Receive Interrupt from Disk I/O Subsystem**:
   1. **Receive Interrupt**: After the disk completes the data transfer, it sends an interrupt to the OS.
   2. **Save Registers and Process State for Other Users**: The OS saves the state information of the currently running other process.
9. **Confirm Interrupt Came from Disk**:
    The OS confirms that the received interrupt was from the disk I/O subsystem.
10. **Update Page Table and Other Tables**:
    1.  **Update Page Table**: Modify the page table to set the valid-invalid bit to valid (V) and record the new physical frame number.
    2.  **Update Other Tables**: Possibly update other related tables to reflect that the page is now in memory.
11. **Wait for CPU to Be Allocated to This Process Again**:
    The OS reassigns the CPU back to the process that caused the page fault.
12. **Restore User Registers, Process State, and New Page Table**:
    1. **Restore User Registers and Process State**: The OS restores the previously saved user registers and process state.
    2. **Restore New Page Table**: Ensure the page table reflects the latest memory mappings.
    3. **Resume the Interrupted Instruction**: Return control to the process and re-execute the instruction that caused the page fault.

In the worst case, handling a page fault involves multiple stages, including capturing the fault, saving state, checking legality, issuing read requests, waiting for disk service, processing interrupts, updating tables, and finally restoring and re-executing instructions. These steps ensure that even under extreme conditions, the operating system can effectively manage and respond to page faults, ensuring program correctness and system efficiency.

### Performance of Demand Paging

## Copy-on-Write

## Page Replacement

## Allocation of Frames

## Thrashing

## Memory-Mapped Files

## Allocating Kernel Memory