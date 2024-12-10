---
layout: post
title: "Cloud Computing Review"
description: "Performing a review of cloud computing."
date: 2024-12-10
feature_image: images/cloud-computing.png
tags: ['cloud-computing']
---

Cloud computing allows companies to store their infrastructures remotely via the internet, ultimately reducing costs and creating value. We will go through a high level overview of cloud computing to prepare for the final exam.

<!--more-->

## Table of Contents

- [Introduction](#introduction)
  - [Cloud Pricing](#cloud-pricing)
  - [Cloud Deployment Models](#cloud-deployment-models)
  - [Cloud Service Models](#cloud-service-models)
- [Virtualization](#virtualization)
  - [Purpose of Virtualization](#purpose-of-virtualization)
  - [Principles of Virtualization](#principles-of-virtualization)
- [Containerization](#containerization)

---

# Introduction

Cloud computing is a model for enabling **ubiquitous**, **convenient**, **on-demand** network access to a shared pool of configurable computing resources(e.g., networks, servers, storage, applications,and services) that can be rapidly provisioned and released withminimal management effort or service provider interaction.

- Ubiquitous: The ability to access computing resources from anywhere in the world.
- Convenient: The ability to access computing resources with minimal effort.
- On-demand: The ability to provision computing resources as needed.

> Define by National Institute of Standards and Technology (NIST), U.S. Department of Commerce

## Cloud Pricing

Cloud Pricing refers to the pricing models and strategies adopted by cloud service providers for their services, such as computing, storage, and networking. Cloud pricing is typically based on a **pay-as-you-go** principle, meaning users pay only for the resources they actually consume. This approach **enhances resource efficiency** and **reduces initial investment costs** for users. Major pricing models include:

- **Pay-as-You-Go**: Users pay based on actual resource usage, often billed by the hour, minute, or second.
- **Reserved Instances Pricing**: Users commit to purchasing a certain amount of resources upfront (typically for one or more years) for discounted rates.
- **Spot Pricing**: A cloud pricing model where service providers offer their unused computing resources at prices lower than on-demand pricing, based on market supply and demand dynamics. This model is suitable for running flexible, fault-tolerant tasks, as services may be interrupted due to fluctuations in resource demand.
- **Tiered Pricing**: Unit prices decrease as usage increases. For instance, storage costs per TB decrease for data exceeding 1TB.

| Attribute             | Pay-as-You-Go                        | Reserved Instances          | Spot Pricing                  | Tiered Pricing                       |
| --------------------- | ------------------------------------ | --------------------------- | ----------------------------- | ------------------------------------ |
| Commitment            | No                                   | Yes                         | No                            | No                                   |
| Pricing               | Hourly/secondly                      | Upfront discount            | Market-based                  | Unit price decrease with usage       |
| Flexibility           | High                                 | Low to Medium               | High                          | High                                 |
| Interruptibility      | Low                                  | Low                         | High                          | Low                                  |
| Cost Saving Potential | Standard                             | Significant (long-term)     | Very High                     | Moderate to High                     |
| Use Cases             | Temporary or unpredictable workloads | Stable, long-term workloads | Elastic, fault-tolerant tasks | Large-scale storage or data transfer |

{% include image_caption.html imageurl="/images/cloud-pricing.png" title="Cloud Pricing" caption="cloud pricing" %}

## Cloud Deployment Models

Cloud Deployment Models refer to the ways in which cloud computing resources are configured and utilized. Based on **ownership**, **management**, and **access permissions**, the primary deployment models are as follows:

- **Public Cloud**: Cloud services are provided by a third-party cloud provider and are available to all users.
- **Private Cloud**: Cloud resources are dedicated to a single organization and can be managed internally or by a third party.
- **Hybrid Cloud**: Combines public and private clouds to leverage the best of both, interconnected via technology like VPNs or dedicated networks.
- **Community Cloud**: Shared and managed by a group of organizations with common requirements (e.g., regulatory or compliance needs).

| Deployment Model    | Characteristics                                                                  | Advantages                                             | Disadvantages                                                        |
| ------------------- | -------------------------------------------------------------------------------- | ------------------------------------------------------ | -------------------------------------------------------------------- |
| **Public Cloud**    | Operated by third-party providers, multi-tenant, pay-as-you-go, open to public   | Low cost, high scalability, provider-managed           | Higher security and privacy risks, limited customization             |
| **Private Cloud**   | Dedicated to a single organization, single-tenant, internally/externally managed | Enhanced security and privacy, high customization      | High cost, limited scalability                                       |
| **Hybrid Cloud**    | Combines public and private clouds, interconnected via technology (e.g., VPN)    | Balances cost and security, workload migration support | High management complexity, requires strong integration capabilities |
| **Community Cloud** | Shared by organizations with common needs (e.g., compliance), resource sharing   | Cost/resource sharing, optimized for community needs   | Limited flexibility and scalability, complex management              |

## Cloud Service Models

Cloud service models are divided into multiple levels according to the level of abstraction and service types provided. The following are common models and their characteristics, advantages and applicable scenarios:

- **Infrastructure-as-a-Service (IaaS)**: Provides virtualized computing resources such as Virtual Machines (VMs), storage, and networking. Users can install any desired software on top of this infrastructure.
- **Platform-as-a-Service (PaaS)**: In addition to infrastructure, it offers development tools, database management, Business Intelligence (BI) services, etc. It simplifies the process of developing and deploying applications.
- **Software-as-a-Service (SaaS)**: Provides a complete software solution via a web browser or client, such as email, chat, and file sharing services.
- **Function-as-a-Service (FaaS)**: A serverless architecture that allows developers to write and deploy individual functions or code snippets. These functions execute in response to triggers such as HTTP requests, database events, etc. The FaaS platform manages the servers and other infrastructure, allowing developers to focus on implementing business logic without worrying about the underlying resource configuration and management.
  - **Serverless Architecture**: Doesn’t mean that there are literally no servers, but it means that developers don’t need to directly manage and maintain servers. All backend services are handled by the cloud provider.
  - **Trigger-based**: Refers to events that can activate the execution of specific functions. For example, if a user uploads a picture to a storage bucket, this may trigger a function to automatically resize the picture.
- **Machine Learning-as-a-Service (MLaaS)**: Provides a suite of tools and services that enable businesses and individuals to leverage machine learning technology without needing an in-depth understanding of its complex algorithms and technical details. MLaaS platforms typically offer data preprocessing, feature extraction, model training, evaluation, and prediction capabilities, with these operations often achievable through simple API calls. This service model significantly lowers the barrier to using machine learning technologies, promoting broader applications and development.

{% include image_caption.html imageurl="/images/cloud-service.png" title="Cloud Service Models" caption="cloud service models" %}

| Service Model  | Characteristics                                        | Advantages                                      | Disadvantages                                  | Use Cases                                    |
| -------------- | ------------------------------------------------------ | ----------------------------------------------- | ---------------------------------------------- | -------------------------------------------- |
| **IaaS**       | Virtualized resources (VMs, storage, networking)       | High flexibility, strong control, pay-as-you-go | Requires expertise, deployment complexity      | Hosting, testing, data storage               |
| **PaaS**       | Development platform (OS, runtime, database, tools)    | Efficient development, automated management     | Low portability, limited customization         | Web apps, APIs, microservices                |
| **SaaS**       | Complete software solutions via browser or client      | No maintenance, multi-device access             | Data privacy concerns, limited customizations  | CRM, collaboration tools, email services     |
| **FaaS**       | Serverless model for running code                      | Low operational cost, elastic, precise billing  | Cold start delay, complex state management     | Event-driven tasks, IoT processing, APIs     |
| **MLaaS**      | Machine learning infrastructure and pre-trained models | Simplified MaaL deployment, cost-effective      | Data privacy issues, limited model flexibility | Image recognition, NLP, predictive analytics |
| **Other XaaS** | Specialized services (e.g., storage, database)         | Focused solutions, cost flexibility             | Vendor dependence, management complexity       | Database services, cloud storage             |

---

# Virtualization

Virtualization technology is an important part of cloud computing and other IT infrastructure services. It allows us to create multiple virtual computing resources, such as servers, storage devices, and networks, which can run on a single physical hardware, thereby improving resource utilization and simplifying management.

{% include image_caption.html imageurl="/images/virtualization-1.png" title="Virtualization" caption="virtualization" %}

## Purpose of Virtualization 

The primary purpose of virtualization is to **enhance efficiency**, flexibility, and scalability. Through virtualization, we can achieve:

- **Optimize Resource Usage**: Multiple OS instances share resources on a single physical machine, avoiding the waste caused by configuring separate hardware for each application.
- **Simplify Management**: Administrators can manage and allocate virtual resources through a centralized platform, reducing the need for direct manipulation of physical hardware.
- **Enhance Security**: Isolation between virtual environments increases system safety and stability, ensuring issues in one VM do not affect others.
- **Rapid Deployment & Migration**: Quickly create new VMs or migrate existing ones between different physical hosts to meet varying workload demands.
- **Disaster Recovery**: Since VMs exist as files, backup and recovery processes are more straightforward.

{% include image_caption.html imageurl="/images/virtualization-2.png" title="Virtualization - Optimize Resource Usage" caption="virtualization - optimize resource usage" %}

## Principles of Virtualization

The **Trap and Emulate** mechanism is a fundamental approach to implementing virtualization, especially in early **full virtualization environments**. The core idea is to <u>allow the guest operating system (Guest OS) to run within its own virtual environment while executing most non-privileged instructions directly on the host CPU as if it were running natively</u>. However, when certain sensitive instructions are encountered, instead of being executed directly, they trigger an exception (a trap), which then <u>transfers control to the Virtual Machine Monitor (VMM or Hypervisor)</u>.

> The ideal is came from Popek & Goldberg (1974)

In computer architecture, instructions can be categorized based on their access level to system resources and the scope of their impact. Here are several main classifications of instructions:

- **Non-privileged Instructions**: These are instructions that can execute in user mode without violating OS security policies. They generally do not alter the global state of the system directly and can run within a VM without VMM intervention.
- **Privileged Instructions**: Privileged instructions can only run in kernel mode or higher privilege levels. They control and manage hardware resources such as memory management, I/O operations, interrupt handling, and can affect overall system stability. In a virtual environment, they must be managed and emulated appropriately by the VMM.
- **Sensitive Instructions**: A special set of instructions that can influence the visible state or behavior of the system even though they may not require privileged execution rights. For instance, certain instructions reading clock values might not be privileged but return results specific to the physical hardware, thus requiring interception by the VMM to provide a virtualized response.

The mechanism is based on the prepromise that <font color="red">all sensetive instructions are privileged instructions, and they can trigger a trap</font>. Just like figure blew:

{% include image_caption.html imageurl="/images/virtualization-3.png" title="Virtualization - Trap and Emulate" caption="virtualization - trap and emulate" %}

If the prepromise is met, the workflow of Trap and Emulate can be described as follows:

1. **Normal Execution**: Most instructions from the guest OS execute directly on the host CPU since they are non-privileged and do not affect the underlying system state.
2. **Trap on Sensitive Instructions**: When the guest OS attempts to execute a sensitive instruction (e.g., modifying CPU states or accessing protected resources), this triggers a trap, causing control to transfer to the VMM.
3. **VMM Intervention**: Upon receiving the trap, the VMM examines what happened and takes appropriate action to emulate the behavior of that instruction. For example, if the guest OS tries to read the timestamp counter, the VMM might provide a virtualized time value.
4. **Return to Guest OS**: Once the VMM has completed emulating the sensitive instruction, it resumes the execution of the guest OS as if nothing had interrupted it.

This method's advantage lies in its ability to achieve virtualization without modifying the guest OS, but it also has drawbacks, particularly concerning performance. Each encounter with a sensitive instruction introduces overhead due to the context switch between the guest OS and the VMM. 

Moreover, because the x86 architecture was not originally designed for virtualization, <font color="red">some critical instructions could not be easily trapped and emulated</font>, necessitating more complex binary translation techniques to handle them.

{% include image_caption.html imageurl="/images/virtualization-4.png" title="Virtualization - Trap and Emulate" caption="virtualization - trap and emulate" %}

Depending on how sensitive instructions are handled and whether hardware support is relied upon, virtualization can be implemented in three main ways:

- **Full Virtualization**: The VMM fully emulates underlying hardware, enabling unmodified guest OSes to run in a virtual environment. Handling sensitive instructions requires **complex binary translation or dynamic compilation**, which can lead to **performance drops**. Early x86 lacked the necessary hardware features for proper handling of sensitive instructions.
- **Paravirtualization**: Guest OSes are **specially designed or modified** to be aware of running in a virtual environment and collaborate with the VMM for more efficient resource management. This reduces the need for complex binary translation and offers better performance but requires kernel-level support, not easily adaptable by all OSes.
- **Hardware-assisted Virtualization**: Modern **processors come with dedicated instruction sets** supporting virtualization, such as Intel VT-x and AMD-V. This approach <u>combines the benefits of full and paravirtualization, offering near-native performance without modifying guest OSes</u>. Hardware-assisted virtualization allows most non-privileged instructions to execute directly, handling privileged instructions via special mechanisms like VMX root/non-root mode switching.

| Feature                              | Full Virtualization | Paravirtualization | Hardware-assisted Virtualization |
| ------------------------------------ | ------------------- | ------------------ | -------------------------------- |
| **Handling privileged instructions** | Binary Translation  | Hypercalls         | Non-Root / Root Mode             |
| **Performance**                      | Good                | Best               | Better                           |
| **Guest OS modifications**           | No                  | Yes                | No                               |
| **Examples**                         | VMware, VirtualBox  | Xen                | Xen, VMware, VirtualBox, KVM     |


---

# Containerization

{% include image_caption.html imageurl="/images/containers.png" title="Containerization" caption="containerization" %}