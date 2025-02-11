---
layout: post
title: "Operating System Security"
description: "High-level overview of Operating System"
date: 2024-10-16
feature_image: images/security.png
tags: ['operating-system', 'security']
---

Operating systems are used to manage resources and perform tasks. Here we will discuss the basics level security of an operating system. 🔐

<!--more-->

---

# Boot Security

**Boot Sequence**

- **Booting** or **Boot** is the action of loading an operating system into memory from a powered-off state. 
- **BIOS** (Basic Input/Output System) is the code stored in a firmware component. When the computer is powered on, it will be executed first.
- **Second Stage Bootloader** is loaded by the BIOS. It handles loading the rest of the operating system into memory and then passes control of execution to the operating system.

🚨 **Warning**: A malicious user could potentially seize execution of a computer at several points in the boot process. 💣

🛠️ **Solution**: To prevent an attacker from initiating the first stages of booting, many computers feature a **BIOS password** that does not allow a second- stage boot loader to be executed without proper authentication.

**Hibernation** 

- **Hibernation** is a method of saving the state of an operating system.
- While going into hibernation, the OS stores the contents of machine’s memory into a **hibernation file** (such as hiberfil.sys) on **disk** so the computer can be quickly restored later.
- Hibernation mode is different from **sleep mode**. The latter only temporarily retains data in RAM without writing it to the hard drive, which allows for faster resumption but can result in data loss if power is interrupted. 

Hibernation provides the advantage of maintaining data integrity even during longer periods of inactivity or power loss, as it saves the system state to the hard drive.

🚨 **Warning**: Once the computer is in hibernation, attackers or unauthorized individuals can physically remove the hard drive and **analyze the hibernation file on another device**. This allows them to bypass operating system-level security measures and directly access the memory data.

🛠️ **Solution**: Enable **full-disk encryption** to protect the hibernation file. Consider **disabling hibernation** if security is a higher priority than convenience. **Implement physical security** measures to prevent unauthorized access to the hardware.

---

# Memory and File System Security

The contents of a computer are encapsulated in its memory and filesystem. Thus, protection of a computer’s content has to start with the protection of its memory and its filesystem. The details will be explained in other posts. Here we will discuss the basic level solutions.

🛠️ **Memory Security Solution**:

- **Memory Isolation**: The operating system ensures that different processes have isolated memory spaces to prevent unauthorized access.
- **Memory Encryption**: Advanced security measures include encrypting sensitive data in memory to protect against physical attacks.
- **Address Space Layout Randomization** (ASLR): This technique randomizes the location of key data in memory, making it harder for attackers to predict and exploit.

🛠️ **File System Security Solution**:
- **File Permissions**: Operating systems provide file permissions to control which users or processes can read, write, or execute specific files.
- **Encryption**: Full-disk encryption (e.g., BitLocker, FileVault) or file-level encryption (e.g., EFS) protects data stored on the hard drive from unauthorized access.
- **Backup and Recovery**: Regularly back up important data and ensure reliable recovery methods to prevent data loss or corruption.
- **Antivirus and Malware Detection**: Use security software to scan and remove malware that could compromise the integrity of the filesystem.

---

# Password Security

The basic approach to guessing passwords from the password file is to conduct a **dictionary attack**, where each word in a dictionary is hashed and the resulting value is compared with the hashed passwords stored in the password file. A dictionary of **500,000** “words” is often enough to discover most passwords.

🚨 **Warning**:  Most users tend to use simple, memorable passwords that are often derived from common words or phrases. The dictionary contains a large number of frequently used terms, including English words, common phrases, names, place names, dates, and more, which are elements commonly used by users when setting up their passwords. 

🛠️ **Solution**: Use **password policy** to ensure that passwords is complex enough. Add a random **salt** value to each password before hashing, which increases the difficulty for attackers to crack the passwords. Temporarily **lock** an account after a certain number of failed login attempts to prevent brute force attacks.

Details about **salting**:

|     | Without Salt                                       | With Salt                                                             |
| --- | -------------------------------------------------- | --------------------------------------------------------------------- |
| 1.  | User enters id $X$ and password $P$.               | User enters id $X$ and password $P$.                                  |
| 2.  | System will find hash value $H$ for $P$ using $X$. | System will find hash value $H$ and salt value $S$ for $P$ using $X$. |
| 3.  | System checks if $H = h(P)$.                       | System checks if $H = h(P, S)$.                                       |

Salt **Increases Search Space Size**:

Assuming that an attacker **cannot find the salt associated with a userid** he is trying to compromise, then the search space for a dictionary attack on a salted password is of size $2^B\times D$ where $B$ is the number of bits of the random salt and $D$ is the size of the list of words for the dictionary attack. Also, even if an attacker can find a salt password for a userid, he **only learns one password**.

---

Related Posts 👇

📑 [Operating System Overview]({{ site.url }}{{ site.baseurl }}/Operating-System-Overview)

📑 [Processes]({{ site.url }}{{ site.baseurl }}/Processes) and [Threads]({{ site.url }}{{ site.baseurl }}/Threads)

📑 [File System]({{ site.url }}{{ site.baseurl }}/File-System)

📑 [Memory Management]({{ site.url }}{{ site.baseurl }}/Memory-Management)