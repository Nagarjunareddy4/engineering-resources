# 🐧 Linux Filesystem

This guide explains everything you need to know about the Linux Filesystem.

If you've ever wondered:

- What is the Linux Filesystem?
- How does Linux organize files?
- What is the Filesystem Hierarchy Standard (FHS)?
- What are file types?
- What are Inodes?
- What are Hard Links and Soft Links?

This guide is for you.

---

# 📑 Table of Contents

1. What is the Linux Filesystem?
2. Filesystem Hierarchy Standard (FHS)
3. Linux Directory Structure
4. Linux File Types
5. Introduction to Inodes
6. Common Interview Questions

---

# 1. What is the Linux Filesystem?

## Answer

The **Linux Filesystem** is the method Linux uses to organize, store, and retrieve files and directories.

Unlike Windows, Linux organizes everything under a **single root directory (`/`)**.

In Linux, almost everything is treated as a file, including:

- Regular Files
- Directories
- Devices
- Processes
- Network Sockets

This unified approach simplifies system management.

---

## Architecture

```text
                /

                │

 ┌──────────────┼──────────────┐

 home         etc            var

 usr          bin            tmp

 dev          proc           opt
```

---

## Key Characteristics

The Linux Filesystem is:

- Hierarchical
- Case Sensitive
- Secure
- Multi-user
- Efficient
- Scalable

---

## Real World Example

A web server stores:

- Configuration → `/etc/nginx`
- Website Files → `/var/www/html`
- Logs → `/var/log/nginx`

Each type of data is stored in a dedicated location.

---

## Benefits

The Linux Filesystem provides:

- Organized Data Storage
- Standard Directory Layout
- Better Security
- Easy Backup
- Efficient File Management

---

## Production Tip

Never store application files randomly. Follow the Linux filesystem hierarchy to simplify maintenance and troubleshooting.

---

## Interview Question

### Question

What is the Linux Filesystem?

### Answer

The Linux Filesystem is a hierarchical structure that organizes and manages files, directories, devices, and system resources under a single root directory.

---

# 2. Filesystem Hierarchy Standard (FHS)

## Answer

The **Filesystem Hierarchy Standard (FHS)** defines the standard directory structure used by Linux distributions.

It ensures consistency across different Linux systems.

---

## FHS Layout

```text
/

├── bin

├── boot

├── dev

├── etc

├── home

├── lib

├── media

├── mnt

├── opt

├── proc

├── root

├── run

├── sbin

├── srv

├── sys

├── tmp

├── usr

└── var
```

---

## Why FHS?

FHS provides:

- Standardization
- Easier Administration
- Better Portability
- Predictable Locations
- Simplified Troubleshooting

---

## Production Example

A DevOps engineer immediately knows:

```text
/var/log
```

contains logs regardless of whether the server is Ubuntu, Debian, Rocky Linux, or RHEL.

---

## Production Tip

Understanding FHS significantly speeds up troubleshooting because you know exactly where configurations, logs, and binaries are located.

---

## Interview Question

### Question

What is FHS?

### Answer

The Filesystem Hierarchy Standard (FHS) defines the standard directory layout used by Linux systems.

---

# 3. Linux Directory Structure

## Answer

Every top-level directory has a specific purpose.

---

## Important Directories

### /

Root directory.

The starting point of the entire filesystem.

---

### /bin

Essential user commands.

Examples:

```text
ls

cp

mv

cat
```

---

### /sbin

System administration commands.

Examples:

```text
fdisk

mount

reboot
```

---

### /etc

Configuration files.

Examples:

```text
/etc/passwd

/etc/hosts

/etc/ssh
```

---

### /home

Home directories for regular users.

Example:

```text
/home/john
```

---

### /root

Home directory for the root user.

---

### /var

Variable data.

Contains:

- Logs
- Mail
- Cache
- Databases

---

### /tmp

Temporary files.

Usually cleared automatically after reboot.

---

### /usr

User applications and libraries.

Contains:

- Programs
- Documentation
- Shared Libraries

---

### /opt

Optional third-party software.

---

### /dev

Device files.

Examples:

```text
/dev/sda

/dev/null

/dev/random
```

---

### /proc

Virtual filesystem containing process and kernel information.

---

### /sys

Provides information about hardware devices and kernel subsystems.

---

## Production Example

A DevOps engineer troubleshooting an application checks:

```text
/var/log
```

for logs and:

```text
/etc
```

for configuration files.

---

## Production Tip

Never modify system files unless you understand their purpose and have a backup.

---

## Interview Question

### Question

What is the purpose of the `/var` directory?

### Answer

The `/var` directory stores variable data such as logs, cache files, databases, and mail.

---

# 4. Linux File Types

## Answer

Linux supports several file types.

Everything in Linux is represented as a file.

---

## File Types

| Symbol | File Type |
|---------|-----------|
| `-` | Regular File |
| `d` | Directory |
| `l` | Symbolic Link |
| `c` | Character Device |
| `b` | Block Device |
| `p` | Named Pipe |
| `s` | Socket |

---

## View File Types

```bash
ls -l
```

Example Output

```text
-rw-r--r--

drwxr-xr-x

lrwxrwxrwx
```

---

## Internal Workflow

```text
File

↓

Filesystem

↓

Kernel

↓

Storage Device
```

---

## Production Example

A database creates:

- Regular Files
- Directories
- Unix Sockets

to store data and communicate between processes.

---

## Production Tip

Recognizing file types quickly helps during system administration and troubleshooting.

---

## Interview Question

### Question

How do you identify file types in Linux?

### Answer

Use the `ls -l` command. The first character of the permissions string indicates the file type.

---

# 5. Introduction to Inodes

## Answer

An **Inode (Index Node)** is a data structure that stores metadata about a file.

The inode does **not** store the filename.

Instead, it stores:

- File Size
- Owner
- Group
- Permissions
- Timestamps
- File Type
- Disk Block Locations

---

## Architecture

```text
Filename

↓

Inode

↓

Disk Blocks
```

---

## View Inode Number

```bash
ls -i
```

Example Output

```text
125678 file.txt
```

---

## Internal Workflow

```text
Filename

↓

Inode Table

↓

Disk Blocks

↓

File Data
```

---

## Production Example

When two files are hard linked, they share the same inode number while appearing as different filenames.

---

## Production Tip

Understanding inodes helps troubleshoot disk usage issues and broken links.

---

## Interview Question

### Question

What is an inode?

### Answer

An inode is a filesystem data structure that stores metadata about a file, including permissions, ownership, timestamps, and the locations of the file's data blocks.

---

## 📌 Quick Revision

| Topic | Key Concept |
|--------|-------------|
| Linux Filesystem | Hierarchical file organization |
| Root Directory | `/` |
| FHS | Standard Linux directory layout |
| `/etc` | Configuration files |
| `/var` | Logs and variable data |
| File Types | Regular, Directory, Link, Device, Socket |
| Inode | Stores file metadata |

---

# 6. Hard Links

## Answer

A **Hard Link** is an additional directory entry that points to the **same inode** as the original file.

Both the original file and the hard link share the same data on disk.

Deleting one filename does **not** delete the actual file data as long as another hard link exists.

---

## Architecture

```text
file1.txt

      │

      ▼

   Inode 1024

      │

      ▼

  File Data

      ▲

      │

file2.txt
```

Both files point to the same inode.

---

## Create a Hard Link

```bash
ln file1.txt file2.txt
```

---

## Verify Inode

```bash
ls -li
```

Example Output

```text
1024 -rw-r--r-- file1.txt

1024 -rw-r--r-- file2.txt
```

Notice both files share the same inode number.

---

## Characteristics

- Shares the same inode
- Shares the same file data
- Cannot span different filesystems
- Cannot link directories (in normal use)

---

## Production Example

A backup process creates hard links to avoid storing duplicate copies of unchanged files, reducing disk usage.

---

## Production Tip

Use hard links when you need multiple filenames referencing the same data within the same filesystem.

---

## Interview Question

### Question

What is a Hard Link?

### Answer

A Hard Link is another filename that points to the same inode and data as the original file.

---

# 7. Symbolic (Soft) Links

## Answer

A **Symbolic Link (Soft Link)** is a special file that stores the **path** to another file or directory.

Unlike a hard link, it has its own inode.

---

## Architecture

```text
shortcut.txt

      │

      ▼

Symbolic Link

      │

      ▼

original.txt

      │

      ▼

File Data
```

---

## Create a Symbolic Link

```bash
ln -s original.txt shortcut.txt
```

---

## View Symbolic Links

```bash
ls -l
```

Example Output

```text
shortcut.txt -> original.txt
```

---

## Characteristics

- Has its own inode
- Can span different filesystems
- Can point to directories
- Breaks if the original file is removed

---

## Hard Link vs Symbolic Link

| Hard Link | Symbolic Link |
|-----------|---------------|
| Shares inode | Has its own inode |
| Cannot cross filesystems | Can cross filesystems |
| Cannot link directories (normally) | Can link directories |
| Remains valid if original filename is deleted | Becomes broken if target is deleted |

---

## Production Example

A web server creates a symbolic link from:

```text
/var/www/current
```

to the latest application release directory, allowing quick deployments by updating the symlink.

---

## Production Tip

Use symbolic links when referencing files or directories across different filesystems or release directories.

---

## Interview Question

### Question

What is the difference between a Hard Link and a Symbolic Link?

### Answer

A Hard Link points to the same inode as the original file, while a Symbolic Link points to the file's path and has its own inode.

---

# 8. Mount Points

## Answer

A **Mount Point** is a directory where a storage device or filesystem is attached to the Linux directory tree.

Unlike Windows, Linux does not use drive letters such as `C:` or `D:`.

Everything is mounted somewhere under the root (`/`) filesystem.

---

## Architecture

```text
Disk

↓

Filesystem

↓

Mount Point

↓

Accessible Files
```

---

## View Mounted Filesystems

```bash
mount
```

or

```bash
findmnt
```

---

## Mount a Filesystem

```bash
sudo mount /dev/sdb1 /mnt/data
```

---

## Unmount a Filesystem

```bash
sudo umount /mnt/data
```

---

## Production Example

A new storage disk is mounted at:

```text
/var/lib/mysql
```

to store MySQL database files.

---

## Production Tip

Configure permanent mounts using:

```text
/etc/fstab
```

to ensure filesystems are automatically mounted after reboot.

---

## Interview Question

### Question

What is a Mount Point?

### Answer

A Mount Point is a directory where a filesystem is attached, making its contents accessible through the Linux directory hierarchy.

---

# 9. Linux Filesystem Types

## Answer

Linux supports multiple filesystem types, each designed for different workloads.

---

## Common Filesystems

| Filesystem | Description |
|------------|-------------|
| ext4 | Default filesystem for many Linux distributions |
| XFS | High-performance filesystem for large workloads |
| Btrfs | Advanced filesystem with snapshots and checksums |
| FAT32 | Compatible with many operating systems |
| exFAT | Large removable storage devices |
| NTFS | Windows filesystem supported by Linux |

---

## View Filesystem Type

```bash
df -T
```

or

```bash
lsblk -f
```

---

## Production Example

Many enterprise Linux servers running RHEL use **XFS**, while Ubuntu commonly uses **ext4** for the root filesystem.

---

## Production Tip

Choose the filesystem based on workload requirements. For example, XFS is well-suited for large files and enterprise servers, while ext4 is a solid general-purpose choice.

---

## Interview Question

### Question

What is the default filesystem used by many Linux distributions?

### Answer

`ext4` is the default filesystem for many Linux distributions, while some enterprise distributions use XFS by default.

---

# 10. Filesystem Commands

## Answer

Linux provides commands to inspect filesystems, mounted disks, and disk usage.

---

## Display Disk Usage

```bash
df -h
```

---

## Display Filesystem Type

```bash
df -T
```

---

## Display Directory Size

```bash
du -sh /var/log
```

---

## List Block Devices

```bash
lsblk
```

---

## Display Filesystem UUIDs

```bash
blkid
```

---

## Display Mounted Filesystems

```bash
findmnt
```

---

## Internal Workflow

```text
Filesystem

↓

Linux Commands

↓

Disk Information

↓

Administrator
```

---

## Production Example

A DevOps engineer checks:

```bash
df -h
```

before deploying an application to verify sufficient disk space is available.

---

## Production Tip

Monitor filesystem usage regularly to prevent services from failing due to full disks.

---

## Interview Question

### Question

Which command displays disk usage in a human-readable format?

### Answer

Use:

```bash
df -h
```

to display filesystem usage in a human-readable format.

---

## 📌 Quick Revision

| Topic | Key Concept |
|--------|-------------|
| Hard Link | Shares the same inode |
| Symbolic Link | Points to a file path |
| Mount Point | Directory where a filesystem is attached |
| ext4 | Common Linux filesystem |
| XFS | Enterprise filesystem |
| `df -h` | Display disk usage |
| `du -sh` | Display directory size |
| `lsblk` | List block devices |
| `findmnt` | Display mounted filesystems |

---

# 11. Disk Usage Analysis

## Answer

Monitoring disk usage is one of the most important responsibilities of a Linux administrator.

A full filesystem can cause:

- Application failures
- Database issues
- Log generation failures
- Service outages

Regular monitoring helps prevent these problems.

---

## Common Commands

### Check Filesystem Usage

```bash
df -h
```

Example Output

```text
Filesystem      Size  Used Avail Use%

/dev/sda1        50G   35G   13G   73%
```

---

### Check Directory Size

```bash
du -sh /var/log
```

---

### Check All Directories

```bash
du -h --max-depth=1
```

---

### Find Large Files

```bash
find / -type f -size +500M
```

---

## Workflow

```text
Filesystem

↓

Disk Usage

↓

Identify Large Files

↓

Clean Up

↓

Free Space
```

---

## Production Example

A production server becomes slow because:

```text
/var/log
```

contains several large log files.

The administrator identifies them using:

```bash
du -sh /var/log/*
```

and archives or removes old logs to recover disk space.

---

## Production Tip

Configure log rotation using **logrotate** to prevent log files from filling the disk.

---

## Interview Question

### Question

How do you check disk usage in Linux?

### Answer

Use `df -h` to check filesystem usage and `du -sh` to check the size of directories.

---

# 12. Common Filesystem Problems

## Answer

Filesystem-related issues are common in production environments.

Early detection helps prevent downtime.

---

## Common Problems

### Full Disk

Symptoms:

- Unable to create files
- Applications fail to write data
- Database errors

---

### Full Inodes

Symptoms:

- Plenty of free disk space
- Cannot create new files

Check:

```bash
df -i
```

---

### Broken Symbolic Links

Example:

```text
logs -> /old/location/logs
```

Target directory removed.

---

### Incorrect Mounts

Filesystem not mounted.

Check:

```bash
findmnt
```

or

```bash
mount
```

---

### Permission Problems

Applications cannot read or write files.

Verify:

```bash
ls -l
```

---

## Production Example

An application fails even though disk space is available.

Investigation shows all inodes are consumed by millions of tiny cache files.

---

## Production Tip

Monitor both disk space (`df -h`) and inode usage (`df -i`) to avoid unexpected filesystem issues.

---

## Interview Question

### Question

Why might a server report "No space left on device" even though disk space is available?

### Answer

The filesystem may have exhausted its available inodes, preventing the creation of additional files.

---

# 13. Common Beginner Mistakes

## Mistake 1

❌ Deleting system directories.

Never remove directories like:

```text
/etc

/bin

/usr

/lib
```

without understanding their purpose.

---

## Mistake 2

❌ Using:

```bash
rm -rf *
```

without verifying the current directory.

Always check:

```bash
pwd
```

before executing destructive commands.

---

## Mistake 3

❌ Ignoring disk usage.

Regularly monitor:

```bash
df -h
```

---

## Mistake 4

❌ Confusing Hard Links and Symbolic Links.

Remember:

- Hard Links share the same inode.
- Symbolic Links reference a path.

---

## Mistake 5

❌ Editing system configuration files without creating backups.

Example:

```bash
cp /etc/fstab /etc/fstab.bak
```

before making changes.

---

## Production Tip

Always create backups before modifying important configuration files or filesystem settings.

---

## Interview Question

### Question

What are common Linux filesystem mistakes?

### Answer

Common mistakes include deleting critical system directories, ignoring disk usage, confusing hard and symbolic links, and modifying configuration files without backups.

---

# 14. Linux Filesystem Best Practices

## Answer

Following best practices improves system reliability and simplifies maintenance.

---

## Organization

✅ Follow the Filesystem Hierarchy Standard (FHS).

---

## Storage

✅ Keep application data separate from system files.

---

## Monitoring

✅ Monitor:

- Disk Usage
- Inode Usage
- Mount Status

---

## Backups

✅ Back up important files regularly.

---

## Cleanup

✅ Remove:

- Old Logs
- Temporary Files
- Unused Packages

---

## Security

✅ Apply appropriate file permissions.

---

## Documentation

✅ Document:

- Mount Points
- Storage Layout
- Backup Procedures

---

## Production Example

A DevOps team:

- Stores application logs in `/var/log`
- Mounts database storage separately
- Automates backups
- Uses monitoring tools to track disk usage

---

## Production Tip

Automate disk usage monitoring with tools like Prometheus, Grafana, or cron-based scripts to detect storage issues before they affect production.

---

## Interview Question

### Question

What are Linux filesystem best practices?

### Answer

Use the standard filesystem hierarchy, monitor storage, back up data, apply proper permissions, and automate maintenance tasks.

---

# 📌 Quick Revision

### Linux Filesystem Structure

```text
/

├── etc

├── home

├── usr

├── var

├── tmp

├── opt

├── proc

└── dev
```

---

### File Types

```text
-

Regular File

d

Directory

l

Symbolic Link

b

Block Device

c

Character Device

p

Named Pipe

s

Socket
```

---

### Hard Link vs Symbolic Link

```text
Hard Link

↓

Same Inode

↓

Same Data

------------------------

Symbolic Link

↓

Different Inode

↓

Points to Path
```

---

### Important Commands

```bash
df -h

df -i

du -sh

lsblk

findmnt

mount

umount

ls -li
```

---

# Summary

After completing this guide, you should understand:

✅ Linux Filesystem

✅ Filesystem Hierarchy Standard (FHS)

✅ Directory Structure

✅ Linux File Types

✅ Inodes

✅ Hard Links

✅ Symbolic Links

✅ Mount Points

✅ Filesystem Types

✅ Filesystem Commands

✅ Disk Usage Analysis

✅ Filesystem Troubleshooting

✅ Best Practices

---

# Interview Quick Revision

| Question | One-Line Answer |
|----------|-----------------|
| What is the Linux Filesystem? | A hierarchical structure used to organize and manage files and directories |
| What is the root directory? | `/`, the top-level directory of the Linux filesystem |
| What is FHS? | The Filesystem Hierarchy Standard defining the Linux directory structure |
| What is an inode? | A data structure that stores file metadata and pointers to data blocks |
| What is a Hard Link? | Another filename pointing to the same inode |
| What is a Symbolic Link? | A special file that points to another file or directory by path |
| What is a Mount Point? | A directory where a filesystem is attached |
| How do you check disk usage? | `df -h` |
| How do you check inode usage? | `df -i` |
| How do you view mounted filesystems? | `findmnt` or `mount` |

---

# 🎉 Linux Filesystem Module Completed

Congratulations! You have completed **Linux Filesystem**, including:

- ✅ Linux Filesystem Fundamentals
- ✅ Filesystem Hierarchy Standard (FHS)
- ✅ Directory Structure
- ✅ File Types
- ✅ Inodes
- ✅ Hard Links
- ✅ Symbolic Links
- ✅ Mount Points
- ✅ Filesystem Types
- ✅ Filesystem Commands
- ✅ Disk Usage Analysis
- ✅ Filesystem Troubleshooting
- ✅ Best Practices

You now have a strong understanding of how Linux stores, organizes, and manages data—an essential skill for Linux Administration, DevOps, Cloud Engineering, and Site Reliability Engineering (SRE).

---

# 🚀 What's Next?

➡️ **03-linux-permissions.md**

In the next guide, you'll learn:

- Linux Users & Groups
- File Ownership
- File Permissions
- `chmod`
- `chown`
- `chgrp`
- `umask`
- Access Control Lists (ACLs)
- SUID, SGID & Sticky Bit
- Production Best Practices
- Common Interview Questions