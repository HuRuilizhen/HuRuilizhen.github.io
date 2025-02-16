---
layout: post
title: "Build Own Web Server Using Go"
description: "Build Your Own Web Server Using Go | Step-by-Step Guide to Creating a Simple and Efficient Web Server"
date: 2025-02-16
feature_image: images/go.jpeg
tags: ['web-development', 'go', 'still-in-progress']
---

In our team, March Studio, we developed a scheduling module that requires concurrency capabilities. Initially, we built this module using Python with the Flask library, but we encountered performance issues. As a result, we decided to create a simple web server using Golang. This module is still under development. This article documents my experience building a web server with Golang, and I hope it can be helpful to others.

<!--more-->

## Table of Contents
- [What is Golang, Why Use It, and How to Start Using It?](#what-is-golang-why-use-it-and-how-to-start-using-it)
  - [Introduction to Golang](#introduction-to-golang)
  - [Start Using Golang](#start-using-golang)
- [Web Server Example with Go](#web-server-example-with-go)
  - [Web Server Explanation](#web-server-explanation)


---

# What is Golang, Why Use It, and How to Start Using It?

## Introduction to Golang

**Go**, also known as **Golang**, is a **statically typed**, **compiled**, and **concurrent-friendly** programming language developed by Google. It aims to address the **pain points of C++** in large-scale software development, such as slow compilation speed, high code complexity, and difficulty in managing concurrent programming. **Go inherits the efficiency of C while simplifying its syntax**, making it easier to write and maintain.

Go has the following notable features:
- **Statically Typed**: Go is statically typed, which means that the type system checks the types of variables at compile time and prevents type errors.
- **Automatic Garbage Collection**: Go provides automatic garbage collection, which frees the developer from worrying about memory management.
- **Single Binary File Compilation**: Go compiles directly into machine code, generating a **stand-alone executable file** without additional dependencies, which makes deployment more convenient.
- **Rich Standard Library**: Go provides a rich standard library that includes **HTTP server**, **JSON parsing**, **database access**, and more, reducing the need for third-party libraries.
- **Built-in Concurrency**: Go natively supports concurrent programming through lightweight threads (**goroutine**) and channels, enabling efficient concurrent processing.

> A goroutine is a **lightweight thread** in the Go programming language. Each goroutine only occupies a **few KB** of memory, while an OS thread usually takes up several MB. Therefore, Go servers can efficiently handle large-scale concurrent requests. For more information, see the [Wikipedia Page on Concurrency in Go](https://en.wikipedia.org/wiki/Go_(programming_language)#Concurrency).

Go is becoming the top choice for web development due to its high performance, simplicity, and maintainability. Go's statically compiled, compiled to a single binary file, has a strong standard library, built-in dependency management, supports **[cross-compilation](#cross-compilation)**, and has a concurrency model that is highly efficient. 

For example, we can write a simple web server through following code:

```go
package main

import (
	"fmt"
	"net/http"
)

func handler(w http.ResponseWriter, r *http.Request) {
	fmt.Fprintf(w, "Hello, Golang Web Server!")
}

func main() {
	http.HandleFunc("/", handler)
	http.ListenAndServe(":8080", nil)
}
```

<a name="cross-compilation"></a> **Cross-compilation** is the process of compiling a program on one computer architecture or operating system to produce an executable file that can run on another architecture or operating system. For example, compiling a program on a Windows computer that can run on Linux or macOS, or compiling a program on an x86 CPU that can run on an ARM architecture (such as a Raspberry Pi or Android device).

Cross-compilation in Go is **very simple and does not require additional toolchains**. Simply set the target platform and architecture through environment variables, and you can compile binaries for different systems.

The syntax for cross-compilation is as follows:

```bash
GOOS=<target operating system> GOARCH=<target architecture> go build -o <output file name>
```

The following table summarizes the common target operating systems and architectures:

<table>
    <tr>
        <th>GOOS (Operating System)</th>
        <th>GOARCH (Architecture)</th>
    </tr>
    <tr>
        <td>Windows</td>
        <td>amd64, 386</td>
    </tr>
    <tr>
        <td>Linux</td>
        <td>amd64, 386, arm, arm64</td>
    </tr>
    <tr>
        <td>Darwin (macOS)</td>
        <td>amd64, arm64 (Apple M1/M2 chip)</td>
    </tr>
    <tr>
        <td>FreeBSD</td>
        <td>amd64, 386</td>
    </tr>
    <tr>
        <td>Android</td>
        <td>arm64, arm</td>
    </tr>
</table>

Although Go does not support compiling multiple target platforms into a single binary file with a single `go build` command, you can use shell scripts or Makefiles to batch compile multiple platforms' executable files. For example, you can use the following bash script to compile multiple platforms' executable files simultaneously:

```bash
#!/bin/bash

APP_NAME="myapp"
PLATFORMS=("windows/amd64" "linux/amd64" "darwin/amd64" "darwin/arm64")

for PLATFORM in "${PLATFORMS[@]}"
do
    GOOS=${PLATFORM%/*}
    GOARCH=${PLATFORM#*/}

    OUTPUT_NAME="${APP_NAME}_${GOOS}_${GOARCH}"
    
    if [ "$GOOS" = "windows" ]; then
        OUTPUT_NAME+='.exe'
    fi

    echo "Compiling for $GOOS/$GOARCH..."
    env GOOS=$GOOS GOARCH=$GOARCH go build -o $OUTPUT_NAME

    if [ $? -ne 0 ]; then
        echo "Error: $GOOS/$GOARCH"
    else
        echo "Done: $OUTPUT_NAME"
    fi
done
```

Although Go's standard library is already powerful, the community has also developed many excellent web frameworks to make development more efficient:
- **[Gin](https://gin-gonic.com/)**: A high-performance web framework that provides features such as routing, middleware, and JSON parsing, making it easier to use than `net/http`.
- **[Echo](https://echo.labstack.com/)**: Similar to Gin, but with better performance, suitable for building RESTful APIs.
- **[Fiber](https://gofiber.io/)**: Based on the fasthttp library, designed for high-performance web servers, suitable for extreme high-concurrency scenarios.

## Start Using Golang

Since my primary development machines run `Ubuntu 24.04.1 LTS x86_6` and `macOS Sonoma 14.6.1 arm64`, we will focus on setting up the environment on these two platforms in this section. Sorry for `Windows` users, but the instructions here might not be applicable to your environment.

We can of course install **Golang** through [official website](https://go.dev/dl/). But I recommend you to install it package manager [Homebrew](https://brew.sh/) for `macOS` or [apt](https://ubuntu.com/server/docs/package-management) for `Ubuntu`. The installation process is very simple:

For `macOS` we use the following command:

```bash
brew install golang
```

For `Ubuntu` we use:

```bash
sudo apt install golang
```

Check the version of Go by running:

```bash
go version
```

And then, boom! You have successfully installed Go and are ready to start using it. For a soomth editing experience, using `vscode` with [Go extension](https://marketplace.visualstudio.com/items?itemName=golang.go) is recommended. But for cool geeks, why not using `astronvim`, see my [previous article]({{ site.url }}{{ site.baseurl }}/Linux-Terminal-Beautify#editor-setup) about `astronvim` setup. And I add following configuration to `mason`:

```plaintext
crlfmt
  Formatter for Go code that enforces the CockroachDB Style Guide.

  installed version v0.3.0                                
  homepage          https://github.com/cockroachdb/crlfmt 
  languages         Go                                    
  categories        Formatter                             
  executables       crlfmt 

go-debug-adapter
    Go debug adapter sourced from the VSCode Go extension.

    installed version v0.44.0                             
    homepage          https://github.com/golang/vscode-go 
    languages         Go                                  
    categories        DAP                                 
    executables       go-debug-adapter

gopls
    gopls (pronounced "Go please") is the official Go language server developed by the Go team. It provides IDE features to any LSP-compatible editor.


    installed version v0.17.1                                     
    homepage          https://pkg.go.dev/golang.org/x/tools/gopls 
    languages         Go                                          
    categories        LSP                                         
    executables       gopls
```

From now on, we have done with the environment setup, let's start coding!

---

# Web Server Example with Go

In this section, we will embark on creating a basic yet functional web server using the Go programming language. However, prior to diving into the coding part, it is essential to gain a thorough understanding of what a web server is, its role in the client-server architecture, and how it efficiently handles requests and responses over the internet.

## Web Server Explanation

In short, a Web Server is a specialized software system designed to **receive**, **process**, and **respond** to **requests** sent over the network by **clients** (usually browsers) via HTTP or HTTPS protocols. It is the foundational component for building websites and web applications. Its primary responsibility is t**o map requests to resources or application logic** and **return the appropriate content** (such as HTML pages, JSON data, images, videos, etc.) to the client. Let's take a closer look at the core components of a web server:

A web server needs to **Listen on a Specific Port** (such as 80 for `HTTP` or 443 for `HTTPS`) for incoming network requests and **Manage Connections**. When a client connects, the server accepts the connection and allocates resources to handle the request. To handle a large volume of concurrent requests, the server must have the ability to efficiently handle multiple connections at the same time. Fortunately, Go's goroutine can easily handle thousands of concurrent requests, making it an ideal choice for developing high-performance web servers.

**Request Parsing and Routing Dispatch** are the core tasks of a web server. A web server needs to parse the HTTP request sent by the client, including the request method (`GET`, `POST`, `PUT`, `DELETE`, etc.), `URL` path, header information, query parameters, and request body data. Then, according to the request's URL and method, the server should dispatch the request to the corresponding handler or business logic. Common practices include designing routing rules, such as mapping the root URL to the homepage, /api/users to user-related APIs, and /static/ to static resources. In Go's `net/http` package, you can use `http.HandleFunc` or third-party frameworks (like `Gin` or `Echo`) to simplify routing configuration.

**Serving Static Resources** is another task of a web server. In addition to dynamic content, web servers need to provide static resources (HTML, CSS, JavaScript, images, videos, etc.) to clients. Servers usually specify a static file directory, and when a request matches a file path in that directory, the server directly returns the file content to the client. Example code (serving static files using the Go standard library):

```go
package main

import (
    "net/http"
)

func main() {

    // Specify the static file directory (e.g. "./static" folder)
    fs := http.FileServer(http.Dir("./static"))
    http.Handle("/static/", http.StripPrefix("/static/", fs))

    http.ListenAndServe(":8080", nil)
}
```

**Dynamic Content Generation** involves calling backend APIs, performing database operations, and template rendering to generate HTML pages. A **template engine** can help **combine data with HTML templates** to generate the final page. Go's standard library provides the `html/template` package for this purpose.

**Middleware Support** is an additional feature of a web server. Middleware involves **functions executed before and after** the actual **processing of requests** to **handle common tasks** such as logging request details (URL, method, response time, etc.), authentication and authorization (verifying user identity or permissions), parsing request data (such as JSON or form data), configuring Cross-Origin Resource Sharing (CORS) settings to allow or restrict cross-domain requests, and compression and caching to improve transmission efficiency and response speed. By using middleware, business logic can be decoupled from common functionalities, resulting in clearer code structure.

**Security and Encryption Support** ensure that the server is **secure**. In production environments, it is essential to enable HTTPS encryption to protect user data. A web server should be able to load SSL/TLS certificates and enable encrypted communication. Additionally, the server should be able to protect against common web attacks such as SQL injection and cross-site scripting (XSS). It should also be able to limit request frequency to prevent denial of service (DDoS) attacks and validate user input. Finally, the server should **provide access** to error **logs** for debugging and monitoring purposes.

**Logging and Monitoring** capabilities are also important to a web server. A web server should be able to log all client requests and provide access to those logs for analysis and error detection. It should also provide performance metrics such as request processing time, concurrent connections, and resource usage. Furthermore, the server should be able to monitor its own performance and alert developers to potential issues. This can be done through various means such as email alerts, dashboard notifications, or integration with external monitoring systems. The server should be able to provide real-time statistics on performance and usage, allowing developers to make data-driven decisions about resource allocation and optimization.

**Extensibility and Integration** enable a web server to be **flexible** and **scalable**. A web server should be extensible through a plugin mechanism, allowing developers to add new features such as authentication mechanisms, logging handlers, or caching engines. The server should be able to integrate with external systems such as databases, caching systems (such as Redis), message queues, and search engines. This allows developers to build a complete business system that can be easily integrated with other services. The server should also provide a well-documented API for developers to use when integrating with external systems. This makes it easier for developers to use the server in their applications and reduces the learning curve associated with using a new system.

In summary, a complete web server is not just a simple HTTP request-response tool, but a feature-rich system with high concurrency, high availability, and high security. It should have the following main features:
- **Network Listening and Concurrent Connection Management**
- **HTTP Request Parsing and Routing Dispatch**
- **Static Resource Serving and Dynamic Content Generation**
- **Middleware Support** (Logging, Authentication, Data parsing, etc.)
- **Security Management and HTTPS Encryption**
- **Logging, Error Monitoring, and Performance Monitoring**
- **Extensibility, Plugins, Load Balancing, Session Management, and Caching Strategies**
