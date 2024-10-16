---
layout: post
title: "Docker Setup"
description: "Some tips on how to setup Docker"
date: 2024-10-08
feature_image: images/deployment.jpg
tags: ['deployment', 'docker']
---

Recently, I've been working on the project `LANTAI` in `MARCH STUDIO` and tried to deploy my application using Docker, but there are some problems that I've encountered. Here are some tips on how to setup Docker.

<!--more-->

---

# What is Docker? 🐳

Imagine you build a cool application and want to deploy it in a server. Everthing goes well in your machine, but when you need to deploy it on the server, it's a disaster! 🚨

> Why it can run in my machine but not on the server? 🤔

I believe something like dependency and different environment might be the cause of the problem. And most of the time, it is **REALLY ANNOYING**!

To avoid this, Docker is a solution to solve the problem. It's a platform that allows you to build and deploy applications in an isolated environment. It provides a container that contains everything you need to run your application, including the code, dependencies, aconfiguration files, running environment. I think you can say it is a virtual machine that has minimal need to run your application.

For more information, see official website [Docker](https://www.docker.com/).

---

# How to Install Docker? ⬇️

I mainly focus on the setup on my developing machine (Macbook Air M2) and testing server (Ubuntu 22.04).

## Install Docker in Apple Silicon Environment 🍎

Homebrew is a package manager for macOS. Highly recommended. See [Homebrew](https://brew.sh/).

First, install Homebrew:

```bash
/bin/bash -c "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/HEAD/install.sh)"
```

After this, install Docker:

```bash
brew install --cask docker
```

This command will download and install `Docker Desktop` for Mac, which includes `Docker Engine`, `Docker CLI client`, `Docker Compose`, `Kubernetes`, and other components, and provides a convenient graphical interface to manage these services.

If you do not use `--cask` and try to use brew install docker, Homebrew will only install the `Docker command-line client` and will not include the full desktop version along with the related GUI management tools. For most users who need a graphical interface to more intuitively operate Docker, installing the full version of `Docker Desktop` is more suitable.

Then, check if Docker is installed correctly:

```bash
docker --version
```

If the output is something like following, it means the installation is successful.

```
Docker version 27.2.0, build 3ab4256
```

Finally, test Docker hello-world:

```bash
docker run hello-world
```

If you get the output like `Hello from Docker!`, it means the environment is ready.

By the way, before you run the test, make sure you have start the `Docker Desktop` application. So that the it can connect to the `docker daemon`.

## Install Docker in Linux Ubuntu Environment 🐧

The testing server is Ubuntu 22.04 provided by `Alibaba Cloud` in Shenzhen. You can get it from [Alibaba Cloud](https://www.aliyun.com/). For some reason, the machine in mainland can't connect to global network. Some tools like `apt` or `pip` or `docker` might not work if we don't set mirror site for them. But luckily, `Alibaba Cloud` provides mirror site by default, some tools like `apt` or `pip` work as expected. However, some tools like `docker` maybe need more steps to set up.

`Alibaba Cloud` provides a very useful tutorial on how to set up the docker environment based on the server they provide. You can get it from [docker tutorial](https://help.aliyun.com/zh/ecs/use-cases/install-and-use-docker-on-a-linux-ecs-instance?spm=5176.21213303.J_qCOwPWspKEuWcmp8qiZNQ.11.27a52f3dqmFtCG&scm=20140722.S_help@@文档@@51853._.ID_help@@文档@@51853-RL_docker-LOC_llm-OR_ser-PAR1_213e390f17290602087986076efc4b-V_3-RE_new3@@cardNew). I just copy the key steps as follows:

- Update apt packages
    ```bash
    sudo apt update
    ```
- Install docker dependencies
    ```bash
    sudo apt-get -y install ca-certificates curl
    ```
- Add the GPG key for the official Docker repository to your system
    ```bash
    sudo install -m 0755 -d /etc/apt/keyrings
    sudo curl -fsSL http://mirrors.aliyun.com/docker-ce/linux/ubuntu/gpg -o /etc/apt/keyrings/docker.asc
    sudo chmod a+r /etc/apt/keyrings/docker.asc
    ```
- Add the Docker repository to apt sources
    ```bash
    echo \
    "deb [arch=$(dpkg --print-architecture) signed-by=/etc/apt/keyrings/docker.asc] http://mirrors.aliyun.com/docker-ce/linux/ubuntu \
    $(. /etc/os-release && echo "$VERSION_CODENAME") stable" | \
    sudo tee /etc/apt/sources.list.d/docker.list > /dev/null
    ```
- Update apt packages
    ```bash
    sudo apt-get update
    ```
- Install docker
    ```bash
    sudo apt-get install -y docker-ce docker-ce-cli containerd.io
    ```
- Check if Docker is installed correctly
    ```bash
    docker --version
    ```
    If you get the output like following, it means the installation is successful.
    ```bash
    Docker version 27.3.1, build ce12230
    ```
- Start docker daemon and set it to start on boot
    ```bash
    sudo systemctl start docker
    sudo systemctl enable docker
    ```
    If you get the output like following, it means the environment is ready.
    ```bash
    root@iZwz9blngzqalt0mglj3h0Z:~# sudo systemctl status docker
    ● docker.service - Docker Application Container Engine
        Loaded: loaded (/usr/lib/systemd/system/docker.service; enabled; preset: enabled)
        Active: active (running) since Tue 2024-10-15 14:10:25 CST; 24h ago
    TriggeredBy: ● docker.socket
        Docs: https://docs.docker.com
    Main PID: 24668 (dockerd)
        Tasks: 28
        Memory: 60.0M (peak: 219.8M)
            CPU: 24.369s
        CGroup: /system.slice/docker.service
                ├─24668 /usr/bin/dockerd -H fd:// --containerd=/run/containerd/containerd.sock
                ├─27622 /usr/bin/docker-proxy -proto tcp -host-ip 0.0.0.0 -host-port 8000 -container-ip 172.18.0.2 -container-port 8000
                └─27629 /usr/bin/docker-proxy -proto tcp -host-ip :: -host-port 8000 -container-ip 172.18.0.2 -container-port 8000
    ```
    Also you can test docker hello-world:
    ```bash
    sudo docker run hello-world
    ```

Since server is in mainland, we need accelerate the network connection. See [accelerate the pulls of docker official images](https://help.aliyun.com/zh/acr/user-guide/accelerate-the-pulls-of-docker-official-images?spm=a2c4g.11186623.4.2.13c962cbXv29lF&scm=20140722.H_60750._.ID_60750-OR_rec-V_1) for more details.

```bash
sudo mkdir -p /etc/docker
sudo tee /etc/docker/daemon.json <<-'EOF'
{
  "registry-mirrors": ["https://<prefix>.mirror.aliyuncs.com"]
}
EOF
sudo systemctl daemon-reload
sudo systemctl restart docker
```

---