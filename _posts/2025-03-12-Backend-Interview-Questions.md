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
- [Operating System Based 操作系统基础](#operating-system-based-操作系统基础)
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

---

# Operating System Based 操作系统基础

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