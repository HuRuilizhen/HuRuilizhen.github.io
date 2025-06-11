---
layout: post
title: "File System"
description: "Basic Concepts of File System"
date: 2024-10-17
feature_image: images/operating-system.jpg
tags: ['operating-system', 'security']
---

File systems are used to manage the storage and organization of data on a computer. Here we will discuss the basic concepts of a file system. 📁

<!--more-->

---

# General Principles 

- **Files and folders** are managed by the operating system
- Applications, including shells, can access files and folders through the **API**
- **Access Control Entry** (ACE): Allow/deny a certain type of access to a file/folder by user/group
- **Access Control List** (ACL): Collection of ACEs for a file/folder
- **File handle** provides an opaque identifier for a file/folder
- File operations: **open** (returns file handle), **close** (invalidates file handle), **read**, **write**, **execve**.
- Hierarchical file organization: **Tree** structure (Windows) or **DAG** structure (Linux). 

For Linux, directories (folders) forms a tree, each directory has links to zero or more files or directories. And here exists the concept of **hard link** and **symbolic link**.

| Feature                  | Hard Link                                                         | Symbolic Link (Symlink)                                              |
| ------------------------ | ----------------------------------------------------------------- | -------------------------------------------------------------------- |
| **Definition**           | A reference to the physical location of a file on disk.           | A special type of file that contains a path to another file.         |
| **Inode**                | Points to the same inode as the original file.                    | Has its own unique inode, which points to the target file's path.    |
| **File Type**            | Appears as a regular file.                                        | Appears as a special file type (symlink).                            |
| **Deletion of Original** | The hard link remains valid even if the original file is deleted. | The symlink becomes broken (dangling) if the target file is deleted. |
| **Cross-Filesystems**    | Cannot span different filesystems.                                | Can span different filesystems.                                      |
| **Permissions**          | Inherits the permissions of the original file.                    | Permissions apply to the symlink itself, not the target file.        |
| **Size**                 | Does not increase the size of the file.                           | Increases the size of the directory containing the symlink.          |
| **Use Case**             | Useful for creating multiple references to the same file.         | Useful for creating aliases or shortcuts to files.                   |
| **Command to Create**    | `ln existing_file new_hard_link`                                  | `ln -s target_file new_symlink`                                      |

## Access Control

**Discretionary Access Control** (DAC) is a security model that allows the **owner** of a resource to decide **who can access** their resources and **what type of access** those users are granted. This model is widely used in many operating systems, such as UNIX, Linux, and Windows, due to its flexibility and ease of use. Key features include:

**Mandatory Access Control** (MAC), on the other hand, is a more stringent access control method that enforces a set of security policies defined by the system. In MAC, both users and files are assigned security labels, and **access is granted based on these labels**. The system enforces these rules, and even the owners of the resources cannot override them.

Default **access control policies** are:

|                | Open Policy                                      | Close Policy                                    |
| -------------- | ------------------------------------------------ | ----------------------------------------------- |
| **Default**    | Allow all access                                 | Deny all access                                 |
| **Exceptions** | Define exceptions to deny access                 | Define exceptions to allow access               |
| **Security**   | Less secure, as all access is allowed by default | More secure, as all access is denied by default |

An **Access Control List** (ACL) for a resource is a sorted list of zero or more **Access Control Entries** (ACEs). An ACE specifies that a set of accesses (e.g., read, execute, or write) to the resources is allowed or denied for a user or group.

## Permissions

Permissions often displayed in compact 10-character notation. To see the full list of permissions, use the `ls -l` command or `eza --long` command.

The output is a formatted list of the files and directories in the directory. Lines for directories in output begin with `d`. The example below lists the permissions of the current directory: `[type bit][user bits][group bits][other bits]`

{% include image_caption.html imageurl="/images/file-system-eza-example.png" title="eza --long" caption="eza --long"%}

Here are some examples of permissions:

| Permissions | Owner                | Group         | Other         |
| ----------- | -------------------- | ------------- | ------------- |
| -rwxr-xr-x  | read, write, execute | read, execute | read, execute |
| -rw-r--r--  | read, write          | read          | read          |
| drwxr-xr-x  | read, write, execute | read, execute | read, execute |

For **directories**, `Read` bit allows listing names of files in directory, but not their properties like size and permissions. `Write` bit allows creating and deleting files within the directory. `Execute` bit allows entering the directory and getting
properties of files in the directory.

Here also exists **sticky bit**. It set on directories, prevents users from deleting or renaming files they do not own. Ignored for everything else. In 10-character display, replaces 10th character (x or -) with t (or T if not also executable)

| Permissions | Description                                          |
| ----------- | ---------------------------------------------------- |
| drwxrwxrwt  | Sticky bit set, full access for everyone             |
| drwxrwx--T  | Sticky bit set, full access by user/group            |
| drwxr--r-T  | Sticky, full owner access, others can read (useless) |

**Set-user-ID** (SUID) Bit: For executable files, enables the program to **run as the file owner**, regardless of the user running it. **Not considered for other file types**. When displayed in 10-character format, replaces the 4th character (x or -) with S (or s if not executable).

| Permissions | Description                                    |
| ----------- | ---------------------------------------------- |
| -rwsr-xr-x  | Setuid, executable by all                      |
| -rwxr-xr-x  | Executable by all, but not setuid              |
| -rwSr--r--  | Setuid, but not executable - not commonly used |

**Set-group-ID** (SGID) Bit: It is a special type of access control that can be set on **executable files** and **directories**. On executable files, the sgid bit causes the program to r**un with the file's group**, regardless of whether the user who runs it is in that group. On directories, the sgid bit causes files created within the directory to **have the same group as the directory**. This is useful for directories shared by multiple users with different default groups. The sgid bit is ignored for everything else. In 10-character display, replaces 7-th character (x or - ) with s (or S if not also executable). Here are some examples of permissions with the sgid bit set:

| Permissions | Description                                                 |
| ----------- | ----------------------------------------------------------- |
| -rwxr-sr-x  | Setgid file, executable by all                              |
| drwxrwsr-x  | Setgid directory; files within will have group of directory |
| -rw-r-Sr--  | Setgid file, but not executable - not useful                |

**Permissions in Octal Notations**: Previous syntax is nice for simple cases,but bad for complex changes. An alternative is to use **Octal Digits** (0-7) from left (most significant) to right (least significant) 
`[special bits][user bits][group bits][other bits]`
- Special bit digit = (4 if setuid) + (2 if setgid) + (1 if sticky)
- All other digits = (4 if readable) + (2 if writable) + (1 if executable)
  
Here are some examples of permissions in octal notation:

| Permissions | Description                               |
| ----------- | ----------------------------------------- |
| 0777        | Read, write, and execute by all           |
| 0755        | Read and execute by all, write by owner   |
| 0644        | Read and write by owner, read by all      |
| 0600        | Read and write by owner, no access by all |

**Unix** permissions are not perfect since **groups are restrictive** and **limitations on file creation**. **Linux** optionally uses POSIX ACLs. It builds on top of traditional Unix permissions. Several users and groups can be named in ACLs. Hence, Linux is possible for finer-grained access control. Each ACL is of the form type: `[name]:rwx`

## Root 

The **root** account is a super-user account, similar to the **Administrator** on Windows. Multiple roots are possible. And file permissions do not restrict root. Although this is dangerous, but necessary, and OK with good practices.

**Some related commands**:

| Command | Description                                                                                                     |
| ------- | --------------------------------------------------------------------------------------------------------------- |
| `su`    | Switch user. Usage: `su [username]`                                                                             |
| `sudo`  | Execute a command with superuser privileges. Usage: `sudo [command]`                                            |
| `chmod` | Change the permissions of a file or directory. Usage: `chmod [permissions] [file/directory]`                    |
| `chgrp` | Change the group ownership of a file or directory. Usage: `chgrp [group] [file/directory]`                      |
| `chown` | Change the owner and/or group ownership of a file or directory. Usage: `chown [owner][:group] [file/directory]` |

---

Related Posts 👇

📑 [Operating System Security](/Operating-System-Security)

📑 [Operating System Overview](/Operating-System-Overview)

📑 [Processes](/Processes) and [Threads](/Threads)

📑 [Memory Management](/Memory-Management)
