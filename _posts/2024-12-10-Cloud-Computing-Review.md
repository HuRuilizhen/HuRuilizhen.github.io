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

| Service Model  | Characteristics                                        | Advantages                                      | Disadvantages                                  | Use Cases                                    |
| -------------- | ------------------------------------------------------ | ----------------------------------------------- | ---------------------------------------------- | -------------------------------------------- |
| **IaaS**       | Virtualized resources (VMs, storage, networking)       | High flexibility, strong control, pay-as-you-go | Requires expertise, deployment complexity      | Hosting, testing, data storage               |
| **PaaS**       | Development platform (OS, runtime, database, tools)    | Efficient development, automated management     | Low portability, limited customization         | Web apps, APIs, microservices                |
| **SaaS**       | Complete software solutions via browser or client      | No maintenance, multi-device access             | Data privacy concerns, limited customizations  | CRM, collaboration tools, email services     |
| **FaaS**       | Serverless model for running code                      | Low operational cost, elastic, precise billing  | Cold start delay, complex state management     | Event-driven tasks, IoT processing, APIs     |
| **MLaaS**      | Machine learning infrastructure and pre-trained models | Simplified MaaL deployment, cost-effective      | Data privacy issues, limited model flexibility | Image recognition, NLP, predictive analytics |
| **Other XaaS** | Specialized services (e.g., storage, database)         | Focused solutions, cost flexibility             | Vendor dependence, management complexity       | Database services, cloud storage             |