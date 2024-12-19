---
layout: post
title: "Firewalls, Tunnels, and Network Intrusion Detection"
description: "Introduction to Firewalls, Tunnels, and Network Intrusion Detection"
date: 2024-11-12
feature_image: images/security.png
tags: ['network', 'security']
---

Firewalls, tunnels, and network intrusion detection are some of the most important concepts in network security. A firewall is a device or software that is used to monitor and control network traffic. It is typically used to protect a network from unauthorized access. A tunnel is a way to connect two networks securely over the internet. Network intrusion detection is used to detect and prevent unauthorized access to a network. In this blog, we will explore each of these concepts in more detail.

<!--more-->

## Table of Contents
- [Firewalls](#firewalls)
  - [Firewall Policies](#firewall-policies)
  - [Firewall Types](#firewall-types)
    - [Stateless Filters](#stateless-filters)
    - [Stateful Filters](#stateful-filters)
    - [Application Layer Firewall](#application-layer-firewall)
- [Tunnels](#tunnels)
  - [Secure Shell (SSH)](#secure-shell-ssh)
  - [IPSec](#ipsec)
  - [Virtual Private Networking (VPN)](#virtual-private-networking-vpn)
- [Network Intrusion Detection](#network-intrusion-detection)
  - [IDS Components](#ids-components)
  - [Types of IDS](#types-of-ids)

---

# Firewalls

A firewall is an integrated collection of security measures designed to prevent unauthorized electronic access to a networked computer system.

A network firewall is similar to firewalls in building construction, because in both cases they are intended to **isolate** one "network" or "compartment" from another.

## Firewall Policies

To protect private networks and individual machines from the dangers of the greater Internet, a firewall can be employed to **filter** incoming or outgoing traffic based on a **predefined set of rules** called firewall **policies**.

Packets flowing through a firewall can have one of three **outcomes**:
- Accepted: permitted through the firewall
- Dropped: not allowed through with **no indication of failure**
- Rejected: not allowed through, accompanied by an attempt to **inform the source that the packet was rejected**

Policies used by the firewall to handle packets are **based on several properties** of the packets being inspected, including the protocol used, such as:
- TCP or UDP
- the source and destination IP addresses
- the source and destination ports
- the application-level payload of the packet (e.g., whether it contains a virus).

There are two **fundamental approaches to creating firewall policies** (or **rulesets**) to effectively minimize vulnerability to the outside world while maintaining the desired functionality for the machines in the trusted internal network (or individual computer)

- **Blacklist approach**: All packets are allowed through except those that fit the rules defined specifically in a blacklist. This type of configuration is more **flexible** in ensuring that **service** to the internal network is **not disrupted** by the firewall, but is naive from a security perspective in that it assumes the network **administrator can enumerate** all of the properties of malicious **traffic**.
- **Whitelist approach**: A safer approach to defining a firewall ruleset is the default-deny policy, in which packets are dropped or rejected unless they are specifically allowed by the firewall.

## Firewall Types

There are three main types of firewalls, which are **stateless**, **stateful**, and **application** firewalls. We will give a brief overview of each type.

### Stateless Filters

If a packet **matches** the packet filter's set of rules, the packet filter will **drop** or **accept** it. It **doesn’t maintain** any remembered **context** (or “state”) with respect to the packets it is processing. Instead, it **treats** each packet attempting to travel through it **in isolation** without considering packets that it has processed previously. It may have to be **fairly restrictive** in order to prevent most attacks.

### Stateful Filters

A stateful firewall maintains records of all connections passing through it and can **determine** if a packet is either the **start** of a new connection, a part of an **existing** connection, or is an **invalid** packet. It tells when packets are part
of **legitimate sessions** originating within a trusted network.

Stateful firewalls maintain tables containing information on each active connection, including the **IP addresses**, **ports**, and **sequence numbers** of packets. Using these tables, stateful firewalls can allow only inbound TCP packets that are in response to a connection initiated from within the internal network.

{% include image_caption.html imageurl="/images/stateful-firewall-example.png" title="Stateful Firewall Example" caption="stateful firewall example" %}

### Application Layer Firewall

An Application Layer Firewall works like a **proxy** it can “understand” certain applications and protocols. It operates at the application level of the OSI model (Layer 7) and can filter traffic based on specific **application data**, rather than just source and destination addresses. By acting as a **proxy**, it can intercept all packets traveling to or from an application, allowing it to enforce security policies based on the actual content of data packets. It has the following features:

- **Content Filtering**: Inspects the payload of packets and enforces rules based on specific content like URLs, keywords, and MIME types.
- **Protocol Analysis**: Understands protocols such as HTTP, FTP, and DNS, allowing it to make more informed decisions about what traffic to allow or deny.
- **User Authentication**: Can require users to authenticate themselves before gaining access to specific applications or resources, adding an extra layer of security.
- **Malware Protection**: Capable of detecting and blocking malware by analyzing data at the application layer.

Application Layer Firewalls are especially useful in environments where security requirements are stringent, as they offer fine-grained control over the data entering and leaving a network.


---

# Tunnels

The contents of **TCP** packets are **not normally encrypted**, so if someone is eavesdropping on a TCP connection, he can often see the complete contents of the payloads in this session. 

One way to prevent such eavesdropping without changing the software performing the communication is to use a **tunneling protocol**.

In such a protocol, the communication (packets sent over the untrusted Internet) between a client and server is **automatically encrypted** (end-to-end encryption and decryption), so that useful eavesdropping is infeasible.

## Secure Shell (SSH)

The SSH protocol provides strong **authentication**, **authorization**, and **encryption** for secure communication between the client and server. SSH has two main components: SSH-1 and SSH-2. SSH-1 was the first version of SSH, while SSH-2 is the current version. SSH-2 is more secure than SSH-1 and offers additional features such as secure TCP/IP connections, secure X11 forwarding, and secure file transfer.

SSH enables secure remote access to servers and other network devices. It is commonly used for secure remote access to servers, secure file transfer, and secure network administration.

The SSH protocol works as follows:

1. **Connection Establishment**: The client initiates a connection to the server by sending a connection request.
2. **Information Exchange**: The client and server exchange information on administrative details, such as supported encryption methods and their protocol version, each choosing a set of protocols that the other supports.
3. **Key Negotiation**: The client and server initiate a secret-key exchange to establish a shared secret session key, which is used to encrypt their communication (but not for authentication). This session key is used in conjunction with a chosen block cipher (typically AES, 3DES) to encrypt all further communications.
4. **Authentication**: The server sends the client a list of acceptable forms of authentication, which the client will try in sequence. The most common mechanism is to use a password or the following public-key authentication method:
   - If public-key authentication is the selected mechanism, the client sends the server its public key.
   - The server then checks if this key is stored in its list of authorized keys. If so, the server encrypts a challenge using the client’s public key and sends it to the client.
   - The client decrypts the challenge with its private key and responds to the server, proving its identity.
5. **Communication**: Once authentication has been successfully completed, the server lets the client access appropriate resources, such as a command prompt. 

## IPSec

PSec defines a set of protocols to provide confidentiality and authenticity for IP packets. Each protocol can operate in one of two modes, **transport** mode or **tunnel** mode.

- In **transport** mode, additional IPsec header information is inserted before the data of the original packet, and only the payload of the packet is encrypted or authenticated.
- In **tunnel** mode, a new packet is constructed with IPsec header information, and the entire original packet, including its header, is encapsulated as the payload of the new packet.

## Virtual Private Networking (VPN)

It is a technology that allows private networks to be safely extended **over long physical distances** by making use of a public network, such as the Internet, as a means of transport.

VPN provides guarantees of data confidentiality, integrity, and authentication, despite the use of an **untrusted network** for transmission.

There are two primary types of VPNs, **remote access VPN** and **site-to-site VPN**.

**Remote access VPNs** allow authorized clients to access a private network that is referred to as an **intranet**. For example, an organization may wish to allow employees access to the company network remotely but make it appear as though they are local to their system and even the Internet itself. To accomplish this, the organization sets up a **VPN endpoint**, known as a **network access server**, or NAS. Clients typically install VPN client software on their machines, which handle **negotiating a connection to the NAS** and **facilitating communication**.

**Site-to-site VPN** solutions are designed to provide a **secure bridge** between two or more physically distant networks. Before VPN, organizations wishing to safely bridge their private networks purchased expensive **leased lines** to directly connect their intranets with cabling.

---

# Network Intrusion Detection

**Intrusions** are the actions aimed at compromising the security of the target (confidentiality, integrity, availability of computing/networking resources)

**Intrusion detection** is the process of **detecting** the identification through intrusion signatures and **report** of intrusion activities.

**Intrusion prevention** is the process of both detecting intrusion activities and managing automatic responsive actions throughout the network

An IDS is designed to detect a number of threats, including the following:
- **Masquerader**: an attacker who is falsely using the identity and/or credentials of a legitimate user to gain access to a computer system or network.
- **Misfeasor**: a legitimate user who performs actions he is not authorized to do.
- **Clandestine user**: a user who tries to block or cover up his actions by deleting audit files and/or system log.

In addition, an IDS is designed to detect automated attacks and threats, including the following:
- **Port scans**: information gathering intended to determine which ports on a host are open for TCP connections
- **Denial-of-service attacks**: network attacks meant to overwhelm a host and
shut out legitimate accesses
- **Malware attacks**: replicating malicious software attacks, such as Trojan horses, computer worms, viruses, etc.
- **ARP spoofing**: an attempt to **redirect** IP traffic in a **local-area** network.
- **DNS cache poisoning**: a pharming attack directed at changing a host’s DNS cache to create a falsified domain-name/IP-address association.

## IDS Components

The **IDS manager** compiles data from the **IDS sensors** to determine if an intrusion has occurred. This determination is based on a set of **site policies**, which are rules and conditions that define probable intrusions. If an IDS manager detects an intrusion, then it sounds an **alarm**.

{% include image_caption.html imageurl="/images/ids-component.png" title="IDS Component" caption="IDS component"%}

The alarm types include:
- **True Positive**: An IDS detects an actual intrusion
- **False Positive**: An IDS detects an intrusion when none exists
- **True Negative**: An IDS correctly determines that no intrusion has occurred
- **False Negative**: An IDS fails to detect an actual intrusion

{% include image_caption.html imageurl="/images/ids-alarm.png" title="IDS Alarm Types" caption="IDS alarm types"%}


The following fields should be included in an IDS event record, as identified by Dorothy Denning in a 1987 paper:

- **Subject**: the initiator of an action on the target; could be a user, a process, or a system component
- **Object**: the resource being targeted, such as a file, command, device, or network protocol
- **Action**: the operation being performed by the subject towards the object; could be a read, write, execute, or other operation
- **Exception-condition**: any error message or exception condition that was raised by this action; could be a file not found error, permission denied error, or other type of error
- **Resource-usage**: quantitative items that were expended by the system performing or responding to this action; could include CPU time, memory usage, network bandwidth, or other resource metrics
- **Time-stamp**: a unique identifier for the moment in time when this action was initiated; could be a timestamp in the form of date and time, or a unique identifier such as a UUID

## Types of IDS

**Rule-Based Intrusion Detection** approach involves creating rules that identify the types of actions that match certain known profiles for an intrusion attack. If the IDS manager sees an event that matches the signature for such a rule, it would immediately sound an alarm, possibly even indicating the particular type of attack that is suspected.

**Statistical Intrusion Detection** approach involves building a profile that is a statistical representation of the typical ways that a user acts or a host is used. Once a user profile is in place, the IDS manager can determine thresholds for anomalous behaviors and then sound an alarm any time a user or host deviates significantly from the stored profile for that person or machine.

---