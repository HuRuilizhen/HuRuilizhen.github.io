---
layout: post
title: "Backend Interview Questions"
description: "This post is a collection of backend interview questions which I have encountered recently."
date: 2025-03-12
feature_image: images/interview.png
tags: ['backend', 'interview', 'still-in-progress']
---

Recently, I am applying for a backend developer summer internship and I have been asked a few questions were quite interesting. This post is a collection of backend interview questions which I have encountered. Hope it can help you when you are finding yourself in the same situation 🌟. Since I am applying positions based in China, I will discuss the questions both in Chinese and English.

最近，我正在寻找后端开发的暑期实习，被问了一些有趣的问题。这篇博客是我遇到的问题的汇总，希望能帮助到需要的人。因为我寻找的是大陆的岗位，所以在本文中会同时使用英文和中文。📚

<!--more-->

## Table of Contents 目录
- [Language Based 语言基础](#language-based-语言基础)
  - [What is Difference between malloc and new?](#what-is-difference-between-malloc-and-new)
  - [Introducing the Standard Template Library (STL) in Cpp](#introducing-the-standard-template-library-stl-in-cpp)
  - [What is the Implementation Principle of Virtual Function in Cpp?](#what-is-the-implementation-principle-of-virtual-function-in-cpp)
- [Database Based 数据库基础](#database-based-数据库基础)
  - [Comparisons between B+ Tree and B Tree](#comparisons-between-b-tree-and-b-tree)
  - [What is Transaction? What is ACID?](#what-is-transaction-what-is-acid)
- [Operating System Based 操作系统基础](#operating-system-based-操作系统基础)
  - [Introducing the Task Scheduling Algorithms. What is the difference of it in Linux and Windows?](#introducing-the-task-scheduling-algorithms-what-is-the-difference-of-it-in-linux-and-windows)
  - [What is the Principle of Process Communication Using Pipes?](#what-is-the-principle-of-process-communication-using-pipes)
- [Network Based 网络基础](#network-based-网络基础)
- [Design Pattern 设计模式](#design-pattern-设计模式)

---

# Language Based 语言基础

## What is Difference between malloc and new?

<font color="red" size="2">Tencent Interview Round 1 腾讯第一轮面试</font>

`malloc` and `new` are key mechanisms for dynamic memory allocation in C/C++, but they have significant differences in their underlying behaviors and usage scenarios. In terms of language, `malloc` is a function from the C standard library but can also be normally called in C++. It returns a void* pointer, requiring manual type specification. On the other hand, `new` is a C++ operator that automatically allocates memory based on type, returning a pointer with an explicitly known type. Here's a simple example to create a pointer to an `int`:

<a name="malloc-vs-new-int-pointer"></a>

```cpp
int* p1 = (int*)malloc(sizeof(int));  // C style, return void*
int* p2 = new int;                    // Cpp style, return int*
```

From this example, it can also be seen that when using `malloc`, the user needs to explicitly specify the number of bytes, whereas new handles this automatically.

When the type is an object, using `malloc`and `free` **only allocates and releases memory** without invoking the object's constructor or destructor, while new and delete do the opposite. At this point, new and delete will first call the constructor or destructor before allocating or releasing memory. When dealing with arrays, `malloc` requires manually calculating the number of bytes needed for the array, and `free` does not require specifying the byte count; `new` and `delete[]` ensure that the constructor and destructor of each element in the array are called.

In terms of error handling, `malloc` returns `Null` when allocation fails, thus requiring manual checking, whereas new throws an exception upon failure, eliminating the need for checking (though we can opt to use nothrow, in which case an exception will result in a return of `nullptr`).

<a name="malloc-vs-new-error-handling"></a>

```cpp
int* p3 = (int*)malloc(10000000000LL); // Check Error
if (!p3) { /* Error Handling */ }

try {
    int* p4 = new int[10000000000LL];  // Throw Exception
} catch (const std::bad_alloc& e) {
    /* Error Handling */
}

int* p5 = new (std::nothrow) int[10000000000LL]; // Without Exception, return nullptr
if (!p5) { /* Error Handling */ }
```

Regarding overload mechanisms, malloc and free cannot be overloaded. However, the more modern new and delete operators can be overloaded to achieve highly customized memory allocation.

`malloc` 和 `new` 是 C/C++ 中用于动态内存分配的关键机制，但它们在底层行为和使用场景上有显著区别。 在语言方面，`malloc` 是C标准库的函数但是在Cpp也可以正常调用，返回的是 `void*` 需要手动指定类型。而 `new` 是 Cpp 运算符，可以根据类型自动分配内存，返回的指针类型明确。以下是一个简单的[例子](#malloc-vs-new-int-pointer)，创造 `int` 的指针。

从这个例子同时我们也可以看到，在使用 `malloc` 的时候，使用者需要显示的指定字节数，而 `new` 会自动完成。

当类型是对象时，使用 `malloc` 和 `free` 仅仅会分配和释放内存的时候而不会调用对象的构造函数和析构函数，而 `new` 和 `delete` 相反。注意这个时候 `new` 和 `delete` 会先调用构造函数或者析构函数，然后再分配和释放内存。当类型是数组的时候，`malloc` 需要手动指定计算数组所需要字节数，`free` 不必指定字节数量；`new` 和 `delete[]` 会确保调用数组中的每一个元素的构造函数和析构函数。

在[错误处理](#malloc-vs-new-error-handling)方面，`malloc` 在无法分配的时候会返回 `Null`，因此需要手动检查，而 `new` 在无法分配的时候会抛出异常，不需要检查 （当然我们也可以选择使用 `nothrow`，这时候发生异常会返回`nullptr`）。

在重载机制上，`malloc` 以及 `free` 是不能进行重载的。而更现代化的 `new` 和 `delete` 是可以进行重载来实现高度自定义的内存分配。

## Introducing the Standard Template Library (STL) in Cpp

<font color="red" size="2">Tencent Interview Round 2 腾讯第二轮面试</font>

The STL (Standard Template Library) is an essential part of the C++ Standard Library. It's not only a reusable component library but also embodies a software architecture encompassing data structures and algorithms. Its core design philosophy centers around the concept of "**separation of data and operations**", achieving highly modularized and generic programming through the collaboration of six major components. Here's a summary of these components:

<a name="stl-components"></a>

| Component  | Function                                                                                       | Examples                     |
| ---------- | ---------------------------------------------------------------------------------------------- | ---------------------------- |
| Containers | Store and manage data                                                                          | vector, list, map            |
| Algorithms | Operate on data (sorting, searching, etc.)                                                     | sort, find, transform        |
| Iterators  | Bridge between containers and algorithms, providing a unified interface for accessing elements | iterator, reverse_iterator   |
| Functors   | Enable functions to have object-like properties, used for customizing algorithm behavior       | less<T>, greater<T>          |
| Adapters   | Encapsulate existing components, altering interfaces or functionalities                        | stack, queue, priority_queue |
| Allocators | Manage memory allocation and deallocation                                                      | allocator<T>                 |

Containers are structures designed for storing and managing data, encapsulating underlying data structures to some extent. They provide a unified data access interface (through iterators) and support automatic memory management (such as dynamic expansion in `vector`). Containers are categorized into sequential containers (`vector`, `list`, `array`, etc.), associative containers (`map`, `set`, etc.), and unordered associative containers introduced in C++11 (`unordered_set`, `unordered_map`, etc.).

The algorithm component is separated from the container component; it operates on the data within containers (like sorting, searching, traversing) without depending on specific container types, using **iterators** instead. These operations are **non-intrusive**, meaning they modify the data within containers without altering the container structure itself. Common usages include:

<a name="stl-algorithms"></a>

```cpp
vector<int> v = {10, 9, 8, 7, 6, 5, 4, 3, 2, 1};
sort(v.begin(), v.end());
auto it = find(v.begin(), v.end(), 5);
vector<int> new_v = transform(v.begin(), v.end(), [](int x) { return x * 2; });
```

Iterators offer a unified interface for accessing container elements, abstracting away the differences in underlying implementations. By functionality, they can be classified into forward iterators (`forward_list`), bidirectional iterators (`map`, `list`), and random access iterators (`vector`, `set`), among others.

Functors are class objects that overload the `operator()`, allowing them to be invoked like functions. They are typically used for customizing algorithm behaviors (such as sorting rules, conditional judgments). Common functors in the standard library include:
- Arithmetic operations: `plus<T>`, `minus<T>`
- Relational comparisons: `less<T>`, `greater<T>`
- Logical operations: `logical_and<T>`, `logical_not<T>`

Adapters primarily function to encapsulate existing components, changing their interfaces or functionalities to adapt them for use in different scenarios or to provide a more uniform interface. Common adapters include:
- Container adapters: `stack`, `queue`, `priority_queue`, which are implemented based on deque or vector.
- Iterator adapters: `reverse_iterator`, enabling reverse traversal of containers.
- Function adapters: `bind`, `not1`, `mem_fn`, wrapping functions into objects for use in algorithms. Note that since C++11, these adapters have largely been replaced by lambda expressions.

Allocators are used for managing memory allocation and deallocation for containers, implementing memory control and optimization. The default allocator is `std::allocator<T>`. Custom allocators can also be defined by overloading `allocate()` and `deallocate()`, facilitating advanced features such as **memory pools**.

STL是Cpp标准库的重要组成部分，不仅是一个可复用的组件库，也是一个包含了数据结构与算法的软件架构。其核心设计理念是 “**数据与操作分离**”，通过六大组件的协作，实现了高度模块化和泛型编程。我们可以对这些组件进行[以下](#stl-components)总结。

容器是一种存储和管理数据的结构，其也在一定程度上封装了底层数据结构。其提供统一的数据访问接口（通过迭代器），也支持自动管理内存（如 `vector` 的动态扩容）。在分类上分为顺序容器（`vector`, `list`, `array` 等），关联容器(`map`, `set` 等)，以及Cpp11后的顺序关联容器（`unordered_set`, `unordered_map`等）

算法组件和容器组件分离，其对容器中的数据进行操作，如排序、查找、遍历等而不**用依赖具体容器**类型，通过**迭代器**操作。其操作也是**非侵入式**的，仅仅是对容器中的数据进行改变而不修改容器的结构。一些常见的用法[如下](#stl-algorithms)。

迭代器提供了访问容器元素的统一接口，屏蔽底层实现差异。按功能可以分为前向迭代器（`forward_list`），双向迭代器（`map`, `list`），随机迭代器（`vector`, `set`）等。

仿函数是一种可以重载 `operator()` 的类对象，可像函数一样调用。它通常用于定制算法行为（如排序规则、条件判断）。在标准库中有以下常用的仿函数：
- 算术运算：`plus<T>`, `minus<T>`
- 关系比较：`less<T>`, `greater<T>`
- 逻辑运算：`logical_and<T>`, `logical_not<T>`

配接器的主要功能是对现有组件进行封装，改变其接口或功能。这样可以使得组件能够在不同的情况下使用，或者是提供了一个更加统一的接口。常见的配接器包括：
- 容器配接器：`stack`, `queue`, `priority_queue`，这些配接器都是基于 deque 或 vector 实现的。
- 迭代器配接器：`reverse_iterator`，这个配接器可以实现反向遍历容器。
- 函数配接器：`bind`, `not1`, `mem_fn`，这些配接器可以将函数包装成一个对象，从而能够在算法中使用。注意 Cpp11 之后，这些配接器多被 Lambda 表达式所取代。

空间配置器用于管理容器的内存分配与释放，实现内存控制与优化。默认配置器：`std::allocator<T>`。也可以自定义配置器，重载 `allocate()` 和 `deallocate()`，实现**内存池**等高级功能。

## What is the Implementation Principle of Virtual Function in Cpp?

<font color="red" size="2">Tencent Interview Round 2 腾讯第二轮面试</font>

In C++, virtual functions are a crucial mechanism for achieving polymorphism, allowing derived classes to override methods from a base class and ensuring the correct function is called at runtime based on the actual object type. First, let's discuss the basic concepts of virtual functions. In C++, if a member function is declared as `virtual` in a base class, then even if we call that function through a base class pointer or reference, the overridden version in the derived class will be invoked instead of the base class version. This effect of dynamic binding is achieved through the **virtual function table** (vtable) and the **virtual pointer** (vptr).

The **virtual function table** is a table storing addresses of virtual functions. Each **class** with virtual functions has a unique vtable. It is essentially an array of pointers, with each element pointing to the implementation of a virtual function for that class. The vtable is created at **compile time** and used during **runtime for function calls**.

The **virtual pointer** is a hidden pointer within each object of a class that contains virtual functions, pointing to the **object’s class's virtual function table**. When an object of such a class is created, its constructor automatically sets the vptr to point to the vtable of that class. Because the vptr is stored inside the object, different objects can have different vtables, enabling **dynamic polymorphism**. Since the **vptr** occupies additional storage space in the object (typically 8 bytes), objects of classes containing virtual functions take up slightly more memory than ordinary class objects.

Additionally, it is worth noting that if a function is a **pure virtual function**, which lacks a concrete implementation, subclasses must override it; otherwise, those subclasses also become abstract classes and cannot be instantiated. An **abstract class** refers to a class that contains at least one pure virtual function and cannot directly create objects.

在 Cpp 中，虚函数是实现多态的重要机制，它允许派生类覆盖基类的方法，并且在运行时根据对象的实际类型调用正确的函数。首先我们来谈论一下虚函数的基本概念，在 Cpp 中如果一个成员函数在基类中声明为 `virtual` 那么即使我们通过基类指针或引用调用该函数，也会调用派生类中重写的版本，而不是基类版本。这种动态绑定的效果就是通过**虚函数表**和**虚指针**实现的。

**虚函数表**是一个存储虚函数地址的表，每个**类**（如果有虚函数）都有一个唯一的虚表。它是一个指针数组，每个元素指向该类的虚函数实现。虚表在**编译时创建**，在程序**运行时用于函数调用**。

**虚指针**是每个含有虚函数的类的对象内部的一个**隐藏指针**，指向该**对象**所属的**类的虚函数表**。当一个类对象被创建时，构造函数会自动设置虚指针指向该类的虚函数表。由于虚指针存储在对象内部，因此不同对象可以有不同的虚函数表，从而实现**动态多态**。由于**虚指针**占据了对象的额外存储空间（通常 8 字节），因此包含虚函数的类的对象会比普通类对象多占用一点内存。

另外值得关注是如果是**纯虚函数**，其没有具体实现，子类必须重写，否则该子类也是抽象类，无法实例化。而**抽象类**指的是至少包含一个纯虚函数的类，这样的类不能直接创建对象。

--- 

# Database Based 数据库基础

## Comparisons between B+ Tree and B Tree

<font color="red" size="2">Tencent Interview Round 2 腾讯第二轮面试</font>

In practical applications, the B+ tree is more suitable for use in **database indexing** due to its stable read and write performance and the characteristic of having an ordered linked list at the leaf nodes. On the other hand, B-trees are sometimes used in scenarios that require **frequent data updates**. From the perspective of similarities, both are **multiway balanced search trees** with a highly balanced nature. Nodes can have multiple child entries, not just two, making them **multi-branch**. In these trees, each node's keys are sorted, and the keys within a node separate the ranges of its child nodes. The time complexity for search, insertion, and deletion operations is $O(\log n)$ for both.

Although their external structures are similar, they differ in where they store data. In a B-tree, each node contains both keys and data, so data can be found in non-leaf nodes, making data updates simpler. In contrast, in a B+ tree, all data is retained in the leaf nodes, which are typically connected in a linked list format, while non-leaf nodes only store key information. Additionally, leaf nodes are linked together via pointers to form a linked list, facilitating full-range scans and sequential access. These differences in data storage lead to variations in search efficiency and application scenarios.

在实际应用中，B+树由于其读写性能稳定和叶子节点的顺序链表特性，使得它更适合用于**数据库索引**。而B树则有时候用于那些需要**频繁对数据进行更新**的场景。从相似性的角度，他们都是都是**多路平衡查找树**，有高度平衡的性质。节点可以拥有多个子项，不仅是两个，因此是**多叉**的。树中的每个节点都是按照键值有序排列的，并且每个节点中的键分隔了其子节点的范围。搜索、插入、删除操作的时间复杂度都是$O(log n)$。

虽然外部结构是类似的，当时存储数据的位置有所不同。在B树中，每个节点都包含键和数据，因此，数据可以在非叶子节点找到，其在更新数据的时候也会更简单；但是在B+树中，所有的数据都保留在叶子节点中，并且通常以链表的方式进行连接，非叶子节点只存储键信息，且叶子节点通过指针串联成一个链表，方便全范围扫描和顺序访问。存储数据上的差异也带来了搜索效率上和应用场景下的差异。

## What is Transaction? What is ACID?

<font color="red" size="2">Tencent Interview Round 2 腾讯第二轮面试</font>

In database systems, a transaction is a logical unit of atomic operations designed to ensure data consistency and integrity. It manages database operations by satisfying the **ACID** properties (Atomicity, Consistency, Isolation, Durability), ensuring that data remains in a correct state even in cases of **system failures** or **concurrent access**. Here's a summary of the characteristics of transactions:
- All operations within a transaction either succeed and are committed entirely, or fail and are rolled back completely. For instance, in a bank transfer, both debiting and crediting must be completed simultaneously or canceled altogether, with no intermediate states. This is known as **atomicity**, which represents the smallest indivisible unit logically.
- **Consistency** means that after executing a transaction, the database must transition from one valid state to another. For example, during a transfer, the total amount in accounts should remain consistent, adhering to business rules.
- The **isolation** of transactions ensures that operations under concurrent scenarios do not interfere with each other, as if they were executed serially. Different isolation levels (such as Read Committed, Repeatable Read) are achieved through **locking mechanisms** or **multi-version control**, preventing issues like dirty reads, non-repeatable reads, etc.
- **Durability** of transactions refers to the fact that once a transaction has been executed, the data in the database will not be lost, even in the event of system failures or crashes. This is realized through **logging** and **log replay** for durability.

In practice, a bank system's transfer operation might involve multiple tables, such as account tables and transfer record tables. Simultaneously, it needs to complete both debiting and crediting without interference from different transfer operations involving different accounts. This scenario serves as a classic example illustrating the characteristics of transactions.

Regarding the **lifecycle** of transactions, it can generally be divided into several phases:
- Active: The transaction is performing operations.
- Partially Committed: Operations have been executed but not yet committed.
- Committed: Successfully completed, changes become permanent.
- Failed: Cannot continue execution and needs to be rolled back.
- Aborted: After rollback, the database returns to its state before the transaction began.

<a name="transaction"></a>
<div class="mermaid">
graph LR
    A[Active] --> B[Partially Committed]
    B --> C[Committed]
    B --> D[Failed]
    D --> E[Aborted]
    E --> A
</div>

Another important trade-off concerning transactions occurs between **isolation levels** and **concurrency issues**. Here are some potential concurrency problems between transactions:
- **Dirty Reads**: Reading uncommitted data from other transactions.
- **Non-repeatable Reads**: The same data read twice within the same transaction yields different results.
- **Phantom Reads**: The number of rows returned by the same query condition changes due to inserts/deletes by other transactions.

To avoid these concurrency issues, trade-offs need to be made regarding the isolation level of transactions. Generally, the higher the isolation level, the fewer concurrency problems occur, but this also leads to poorer transaction performance. Transaction isolation levels can be categorized as follows:
- **Read Uncommitted**: Offers the best concurrency performance for transactions but may lead to dirty reads.
- **Read Committed**: Avoids dirty reads but may still result in non-repeatable reads.
- **Repeatable Read**: Ensures that multiple reads within the same transaction yield consistent results (default in MySQL).
- **Serializable**: Provides full isolation, implemented through table locking, resulting in the lowest performance.

To achieve various transaction characteristics, the following mechanisms can be utilized:
- **Logging**: Records transaction operations (such as Redo/Undo logs) for fault recovery.
- **Locking**: Controls concurrent access (e.g., row locks, table locks).
- **Multi-Version Concurrency Control** (MVCC): Implements non-blocking reads via data snapshots (e.g., `PostgreSQL`, `MySQL`, `InnoDB`).

Recommended practices include using **short transactions** whenever possible to reduce lock contention and improve concurrent performance; reasonably selecting **isolation levels** to balance consistency and performance; and paying attention to exception handling to avoid situations of **partial commitment**. 

数据库系统中的事务是一组**原子性操作的逻辑单元**，用于确保数据的一致性和完整性。它通过满足**ACID**特性（原子性、一致性、隔离性、持久性）来管理对数据库的操作，使得即使在**系统故障**或**并发访问**的情况下，数据仍能保持正确状态。关于事务的特性，我们可以进行以下总结：
- 事务中的所有操作要么全部成功提交，要么全部失败回滚。例如，银行转账中扣款和加款必须同时完成或同时取消，不存在中间状态。这就是所谓的**原子性**，其是逻辑上不可再分化的最小单元。
- **一致性**是指事务执行后，数据库必须从一个有效状态转换到另一个有效状态。例如，转账前后账户总金额需保持一致，符合业务规则。
- 事务的**隔离性**确保了，数据库在并发场景下的操作不会互相干扰，如同串行执行。通过**锁机制**或**多版本控制**实现不同隔离级别（如读已提交、可重复读），避免脏读、不可重复读等问题。
- 事务的**持久性**是指事务执行后，数据库中的数据不会丢失，即使在系统故障或系统崩溃时。通过**日志记录**和**日志回放**实现事务的持久性。

在实践中，银行系统的转账操作可能会涉及多个表，如账户表、转账记录表等。同时转账操作也需要同时完成扣款和加款。并且不同的转账操作可能会涉及不同的账户，因此需要避免相互干扰。这个场景是说明事务特性的一个经典的例子。

关于事务的**生命周期**，可以大概分成[以下](#transaction)几个阶段：
- 活动状态（Active）：事务正在执行操作。
- 部分提交（Partially Committed）：操作执行完成，但尚未提交。
- 提交（Committed）：成功完成，修改永久生效。
- 失败（Failed）：无法继续执行，需回滚。
- 中止（Aborted）：回滚后，数据库恢复到事务开始前的状态。

而关于事务的另外一个比较重要的权衡发生在 **隔离级别** 和 **并发问题** 上。我们先来谈谈事务之间可能会存在以下并发问题:
- 脏读：读取到其他事务未提交的数据。
- 不可重复读：同一事务内两次读取同一数据结果不同。
- 幻读：同一查询条件返回的行数因其他事务插入/删除而改变。

为了避免这些并发问题，需要在事务的隔离级别上做出权衡。一般来说隔离级别越高，并发问题越少，但是事务的性能也会越差。事务的隔离级别可以分为以下几个级别：
- 读未提交：此时事务的并发性能最好，但可能会导致脏读。
- 读已提交：避免脏读，但可能不可重复读。
- 可重复读：保证同一事务内多次读取结果一致（MySQL默认）。
- 可串行化：完全隔离，通过锁表实现，性能最低。

为了实现事务的各个特性，可以通过以下机制：
- 日志：记录事务操作（如Redo/Undo日志），用于故障恢复。
- 锁：控制并发访问（如行锁、表锁）。
- 多版本并发控制：通过数据快照实现非阻塞读（如`PostgreSQL`, `MySQL`, `InnoDB`）。

以上，推荐的实践方式是尽可能使用**短事务**，来减少锁竞争，提升并发性能；同时合理选择**隔离级别**，权衡一致性与性能；也需要关心捕获异常的处理，避免**部分提交**的情况。

---

# Operating System Based 操作系统基础

## Introducing the Task Scheduling Algorithms. What is the difference of it in Linux and Windows?

<font color="red" size="2">Tencent Interview Round 2 腾讯第二轮面试</font>

The task scheduling algorithm of an operating system is a core mechanism for managing CPU resource allocation, primarily used to determine the execution order of multiple processes/threads. Different systems adopt various scheduling strategies based on their design goals (such as real-time performance, fairness, throughput). Here are brief introductions to some simple task scheduling algorithms:

**First-Come, First-Served (FCFS)** is a straightforward task scheduling algorithm that allocates the CPU according to the order in which processes arrive at the ready queue. It features simplicity and fairness but can lead to the "convoy effect," where shorter jobs are blocked by longer ones.

**Shortest Job Next (SJN)** schedules processes based on their **estimated run time**, giving priority to those with the shortest expected runtime. This algorithm optimizes average waiting time but requires **prior knowledge of job durations** and is **unfriendly to long jobs**.

**Round Robin (RR)** assigns each process a fixed time slice (e.g., 10ms), after which the process must requeue. This approach emphasizes strong fairness and is suitable for **interactive systems**, though frequent context switching may reduce efficiency.

**Priority Scheduling** determines the scheduling order based on the priority of processes, which can be statically or dynamically adjusted. A downside of this method is that low-priority processes might suffer from "starvation."

**Multilevel Feedback Queue** is an algorithm designed to balance **response time and throughput** by placing processes into multiple queues based on their priority levels, allowing dynamic promotion or demotion between queues. This algorithm is commonly used in actual systems like Windows.

As for Linux, its scheduler has undergone several evolutions, with the current mainstream being the **Completely Fair Scheduler (CFS)**, which emphasizes fairness and throughput. It performs excellently in server environments and high-concurrency scenarios, especially in **multi-core settings**, although it may not respond as swiftly to interactive tasks as Windows does.

操作系统的任务调度算法是管理CPU资源分配的核心机制，主要用于决定多个进程/线程的执行顺序。不同系统根据设计目标（如实时性、公平性、吞吐量）采用不同的调度策略。简单的任务调度算法分别为：

**先来先服务**是一种简单的任务调度算法，根据进程到达就绪队列的顺序分配CPU。其特点是简单、公平，但可能导致“长作业阻塞短作业”。

**短作业优先**则是根据作业的**预计运行时间**来确定调度顺序的算法，优先执行预计运行时间最短的进程。该算法可以使得平均等待时间最优，但需要**预知作业时间**，对**长作业不友好**。

**轮转调度**则是每个进程分配固定时间片（如10ms），时间片用完则重新排队的算法。该算法的特点是公平性强，适合**交互式系统**，但频繁上下文切换可能降低效率。

**优先级调度**是根据进程的优先级来确定调度顺序的算法，优先级可以静态或动态调整。该算法的问题是低优先级进程可能“饿死”。

**多级反馈队列**是一种综合**权衡响应时间和吞吐量**的算法，将进程按优先级分到多个队列，队列间可动态升降级。实际系统（如Windows）常用该算法。

而Linux的调度器经过多次演进，目前主流是**完全公平调度器**，强调公平性和吞吐量。其适合服务器和高并发场景，尤其在**多核环境**下表现优异，但对交互式任务的响应略逊于Windows。

## What is the Principle of Process Communication Using Pipes?

<font color="red" size="2">Tencent Interview Round 1 腾讯第一轮面试</font>

A **process** is the basic unit for operating system resource allocation, with each process having its own **independent** address space. Therefore, inter-process communication requires mechanisms provided by the operating system to be implemented, such as: pipes, message queues, shared memory, signals, and sockets. The essence of these communication methods is that the **kernel** acts as an intermediary, setting aside a region in **kernel space** for processes to **exchange data**. There are some core issues in process communication, namely:
- Data isolation: Processes have independent address spaces and cannot directly access each other's memory.
- Synchronization and mutual exclusion: When multiple processes access shared resources simultaneously, competition conditions must be avoided.
- Performance and efficiency: Different communication methods vary greatly in speed and resource consumption.

Additionally, let's mention the issue of communication between threads. Threads within the same process **share the address space and resources** of the process. Therefore, thread communication can be directly achieved through shared memory without kernel intervention. However, attention should be paid to **shared data issues** and **synchronization/mutual exclusion problems**.

Pipes mentioned in the question are a **half-duplex** communication mechanism where data flows in one direction at a time. They are implemented based on a **kernel buffer**, with both parties communicating through **file descriptors** to read and write data.

> **Half-duplex**: Information transmission is bidirectional; however, at any given moment, only one party can transmit data while the other receives.

An **anonymous pipe** is the simplest form of a pipe, commonly used for communication between related processes (such as parent-child processes). Generally, the usage process of an anonymous pipe is as follows:
- First, perform a `pipe(int pipefd[2])` system call to create an anonymous pipe. `pipefd[0]` is used for reading, and `pipefd[1]` for writing. It is a circular buffer in the kernel, with a default size of 64KB (which may vary depending on the operating system).
- During data flow, data is written into the circular buffer via `pipefd[1]` and read from and removed from the circular buffer via `pipefd[0]`. If the pipe is empty, the read operation will block until data is written. If the pipe is full, the write operation will block until data is read.
- When all write ends are closed, the read end will read an **end-of-file marker**. When all read ends are closed, the write end will receive a `SIGPIPE` signal.

It should be noted that although **anonymous pipes** can facilitate bidirectional data flow, they are generally used for unidirectional data flow in practice. The following code demonstrates a simple example of using an anonymous pipe:

<a name="anonymouspipe"></a>

```cpp
#include <string.h>

int main() {
    int pipefd[2];
    pid_t pid;
    char buf[1024];

    // Create Pipe
    if (pipe(pipefd) == -1) {
        perror("pipe");
        return 1;
    }

    // Create Child Process
    pid = fork();
    if (pid < 0) {
        perror("fork");
        return 1;
    }

    // Child Process
    if (pid == 0) {
        close(pipefd[1]);
        read(pipefd[0], buf, sizeof(buf));
        printf("Child received: %s\n", buf);
        close(pipefd[0]);
    } 
    // Father Process
    else {
        close(pipefd[0]);
        const char *msg = "Hello, child!";
        write(pipefd[1], msg, strlen(msg) + 1);
        close(pipefd[1]);
    }

    return 0;
}
```

If we consider using pipes or persistent information channels between any two processes, then we would need a **named pipe**. A named pipe is a **special file** that allows communication between unrelated processes. It is identified by a **pathname in the filesystem**. Its implementation process is almost identical to that of an anonymous pipe, except there are some differences in **initializing the pipe** and **opening the pipe**:
- Initializing the pipe requires using the `mkfifo` command or performing a `mkfifo()` system call.
- Opening the pipe file uses the `open()` system call, with the read end using the `O_RDONLY` flag, the write end using the `O_WRONLY` flag, or either using the `O_RDWR` flag.

First, you can execute the `mkfifo test_fifo` command in the command line to create a named pipe file called `test_fifo`. Next, you can use the `open` function to open this file and then perform data read/write operations. For example, in one program, you might write data like this:

<a name="namedpipe write"></a>
```cpp
#include <stdio.h>
#include <fcntl.h>
#include <sys/stat.h>
#include <unistd.h>
#include <string.h>

int main() {
    int fd;
    const char *fifo = "test_fifo";
    const char *msg = "Hello, FIFO!";

    fd = open(fifo, O_WRONLY);
    write(fd, msg, strlen(msg) + 1);
    close(fd);

    return 0;
}
```

In another program, you could read the data like this:

<a name="namedpipe read"></a>
```cpp
#include <stdio.h>
#include <fcntl.h>
#include <sys/stat.h>
#include <unistd.h>

int main() {
    int fd;
    char buf[1024];
    const char *fifo = "myfifo";

    fd = open(fifo, O_RDONLY);
    read(fd, buf, sizeof(buf));
    printf("Received: %s\n", buf);
    close(fd);

    return 0;
}
```

**进程**是操作系统资源分配的基本单位，每个进程都有**独立**的地址空间。因此，进程之间的通信需要通过操作系统提供的机制来实现。比如：管道，消息队列，共享内存，信号以及套接字。这些通信方式的本质是**内核**充当中间人，在**内核空间**中开辟一块区域，供进程之间**交换数据**。进程通讯也存在一些核心问题，即：
- 数据隔离：进程的地址空间是独立的，无法直接访问对方的内存。
- 同步与互斥：多个进程同时访问共享资源时，需要避免竞争条件。
- 性能与效率：不同的通信方式在速度和资源消耗上有较大差异。

另外再提一下线程之间的通讯问题。同一进程内的线程**共享进程的地址空间和资源**。因此，线程之间的通信可以直接通过共享内存来实现，无需内核介入。但是需要注意一下**共享数据问题**和**同步互斥问题**。

问题中的管道，是一种**半双工**的通信机制，数据只能单向流动。它是**基于内核的缓冲区**实现的，通信双方通过**文件描述符**来读写数据。

> **半双工**：信息的传输是双向的；但是在任意时刻，进行信息交换的双方只有一方能进行数据传输，另一方只能进行数据接收。

**匿名管道**是管道的最简单形式，通常用于具有亲缘关系的进程间通信（如父子进程）。一般而言进行匿名管道的使用过程如下：
- 首先进行`pipe(int pipefd[2])`的系统调用，创建匿名管道。`pipefd[0]`用于读，`pipefd[1]`用于写。其在内核中是一个环形缓冲区，默认大小是64KB（因为操作系统的不同而不同）。
- 数据流动时，数据会从`pipefd[1]`写入环形缓冲区，从`pipefd[0]`读取并且移除环形缓冲区中的数据。如果管道为空，读操作会阻塞，直到有数据写入。如果管道已满，写操作会阻塞，直到有数据被读取。
- 当所有写端关闭时，读端会读到**文件结束符**。当所有读端关闭时，写端会收到 `SIGPIPE` 信号。

需要注意的是，虽然**匿名管道**是可以进行双向数据流动的，但是实践中一般只是用于单向数据流动的过程。下面的[代码](#anonymouspipe)展示了一个使用匿名管道的简单例子。

如果我们考虑在任意两个进程之间使用管道或者持久化信息通路，那么我们就会需要**命名管道**了。命名管道是一种**特殊的文件**，可以在不相关的进程之间进行通信。它通过**文件系统中的路径名**来标识。其实现过程与匿名管道几乎一样，只是在**初始化管道**和**打开管道**的时候有一些区别：
- 初始化管道的时候需要使用 `mkfifo` 命令或者进行 `mkfifo()` 系统调用。
- 通过`open()`系统调用来打开管道文件，读端用``O_RDONLY`标志，写端用`O_WRONLY`标志，或者用`O_RDWR`标志。

首先可以在命令行下执行 `mkfifo test_fifo` 命令，这样会创建一个名为 `test_fifo` 的命名管道文件。接下来可以使用 `open` 函数来打开这个文件，然后进行数据的读写操作。例如在一个程序中我们[这样](#namedpipe write)写入数据，在另外一个程序中可以[这样](#namedpipe read)读取数据。

---

# Network Based 网络基础

---

# Design Pattern 设计模式

---

Related Posts / Websites 👇

- 📑 [Ray - Processes]({{ site.url }}{{ site.baseurl }}/Processes) and [Ray - Threads]({{ site.url }}{{ site.baseurl }}/Threads)