---
layout: post
title: "Docker Quick Start"
description: "A quick start on how to use Docker"
date: 2024-10-09
feature_image: images/deployment.jpg
tags: ['deployment', 'docker']
---

Last [post](https://huruilizhen.github.io/Docker-Setup), we finished the installation of Docker. It's time to start using Docker. Here are some tips on how to use Docker. With basic knowledge of Docker, it should be easy to use. 

<!--more-->  

---

# Basic Concepts of Docker 🐳

To start using Docker, you need to get familiar with the following concepts:


## 1. Image 📀

- **Definition**: A Docker image is a lightweight, standalone, executable software package that includes everything needed to run a piece of software: code, runtime, libraries, environment variables, and configuration files. Just viewing the image as a virtual machine with minimal need to run your application. Images are read-only, meaning they cannot be changed once created. 
- **Building**: Images can be built using a `Dockerfile`, which contains all the instructions such as the base image selection, file copying, environment variable setting, etc. The `docker build` command is used to build an image from a `Dockerfile`.
- **Layering**: Docker images use a layered filesystem, where each layer represents a change in the image. This design improves disk usage and performance and allows for fast creation of new images.

## 2. Container 🫙

- **Definition**: A container is a running instance of an image. Users can start multiple containers based on the same image. Each container is isolated, with its own process space, network interfaces, and storage volumes. Image is like "Class" and Container is like "Object".
- **Lifecycle**: The states of a container include creation, starting, pausing, stopping, and removal. Containers are started with `docker run`, stopped with `docker stop`, and removed with `docker rm`.
- **Interactivity**: You can enter a running container to view or modify its contents. This is typically done using `docker exec` or by specifying a command directly with `docker run`.

## 3. Repository 📦

- **Definition**: A repository is a place to store and manage Docker images. It can be public (like Docker Hub) or private (self-hosted or third-party services).
- **Functionality**: Users can upload their images to a repository and download images shared by others. This is crucial for team collaboration and continuous integration/deployment.

---

# Build a Docker Image 🐳

Before building a Docker image, you need to create a `Dockerfile`. Here list some grammar of `Dockerfile`:

| Instruction  | Description                                                    | Example                                             |
| ------------ | -------------------------------------------------------------- | --------------------------------------------------- |
| `FROM`       | Set a base image for subsequent instructions                   | `FROM nginx:alpine`                                 |
| `RUN`        | Execute a command in the shell                                 | `RUN apk add --no-cache gcc musl-dev linux-headers` |
| `COPY`       | Copy a file or a directory from the build context to the image | `COPY index.html /usr/share/nginx/html/`            |
| `WORKDIR`    | Set the working directory in the container                     | `WORKDIR /tmp`                                      |
| `EXPOSE`     | Expose a port from the container to the host                   | `EXPOSE 80`                                         |
| `CMD`        | Set the default command to run when starting a container       | `CMD ["nginx", "-g", "daemon off;"]`                |
| `ENTRYPOINT` | Set the default entrypoint for a container                     | `ENTRYPOINT ["docker-entrypoint.sh"]`               |
