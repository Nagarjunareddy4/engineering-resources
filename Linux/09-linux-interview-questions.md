# 🎯 Linux Interview Questions

This guide contains the most frequently asked Linux interview questions for:

- Linux Administrator
- DevOps Engineer
- Cloud Engineer
- Site Reliability Engineer (SRE)
- Platform Engineer

The questions are divided into:

- Beginner
- Intermediate
- Advanced
- Scenario-Based
- Troubleshooting

---

# 📑 Table of Contents

1. Beginner Linux Questions
2. Intermediate Linux Questions
3. Advanced Linux Questions
4. Scenario-Based Questions
5. Troubleshooting Questions
6. DevOps & Cloud Linux Questions

---

# Part 1 – Beginner Linux Interview Questions

---

# 1. What is Linux?

### Answer

Linux is an open-source, Unix-like operating system kernel that manages hardware resources and allows applications to run efficiently.

---

# 2. Why is Linux widely used in DevOps?

### Answer

Because Linux is:

- Stable
- Secure
- Open Source
- Highly Customizable
- Cloud Friendly
- Automation Friendly

Most cloud platforms and Kubernetes clusters run on Linux.

---

# 3. What is a Kernel?

### Answer

The Kernel is the core component of the operating system that manages:

- CPU
- Memory
- Devices
- File Systems
- Processes

---

# 4. What is Shell?

### Answer

A Shell is a command-line interpreter that allows users to interact with the operating system.

Example:

```text
Bash

Zsh

Sh
```

---

# 5. What is Bash?

### Answer

Bash (Bourne Again Shell) is the default shell used by most Linux distributions for command execution and scripting.

---

# 6. What is the difference between Linux and Unix?

### Answer

| Linux | Unix |
|--------|------|
| Open Source | Mostly Proprietary |
| Community Driven | Vendor Controlled |
| Runs on Many Platforms | Mostly Enterprise Systems |

---

# 7. What is the Root User?

### Answer

The Root user is the superuser with unrestricted administrative privileges.

UID:

```text
0
```

---

# 8. What is a Regular User?

### Answer

A Regular User has limited permissions and performs daily tasks without full administrative access.

---

# 9. What is a System User?

### Answer

A System User is created for services and applications.

Examples:

```text
nginx

mysql

www-data

postgres
```

---

# 10. What is a Process?

### Answer

A Process is a running instance of a program.

Each process has:

- PID
- Memory
- CPU Allocation
- State

---

# 11. What is PID?

### Answer

PID (Process ID) is a unique number assigned to every running process.

---

# 12. What is a Zombie Process?

### Answer

A Zombie Process has finished execution but still exists in the process table because its parent has not collected its exit status.

---

# 13. What is a Daemon Process?

### Answer

A Daemon is a background process that provides system or application services.

Examples:

```text
sshd

nginx

cron
```

---

# 14. What is the Linux Filesystem?

### Answer

The Linux Filesystem is a hierarchical directory structure used to organize and store files.

Root directory:

```text
/
```

---

# 15. What is the purpose of `/etc`?

### Answer

It stores system-wide configuration files.

Examples:

- passwd
- fstab
- sshd_config
- hosts

---

# 16. What is `/var` used for?

### Answer

Stores variable data like:

- Logs
- Mail
- Cache
- Databases

---

# 17. What is `/home`?

### Answer

Contains home directories for regular users.

Example:

```text
/home/john
```

---

# 18. What is the difference between Hard Link and Symbolic Link?

### Answer

Hard Link

- Same inode
- Cannot span filesystems

Symbolic Link

- Different inode
- Points to file path
- Can span filesystems

---

# 19. What are Linux Permissions?

### Answer

Permissions control:

- Read
- Write
- Execute

for:

- Owner
- Group
- Others

---

# 20. What does chmod do?

### Answer

Changes file or directory permissions.

Example

```bash
chmod 755 script.sh
```

---

# 21. What does chown do?

### Answer

Changes the owner of a file or directory.

Example

```bash
chown user:filegroup file.txt
```

---

# 22. What is umask?

### Answer

Defines the default permissions assigned to newly created files and directories.

---

# 23. What is SSH?

### Answer

SSH (Secure Shell) is a secure protocol used for encrypted remote login and administration.

---

# 24. What is LVM?

### Answer

LVM (Logical Volume Manager) provides flexible storage management and allows logical volumes to be resized more easily than traditional partitions.

---

# 25. What is Swap Space?

### Answer

Swap Space is disk space used as virtual memory when physical RAM becomes full.

---

# 📌 Quick Revision

| Question | One-Line Answer |
|----------|-----------------|
| What is Linux? | Open-source Unix-like operating system |
| What is Kernel? | Core component managing hardware and software resources |
| What is Shell? | Command interpreter |
| What is Bash? | Default Linux shell |
| What is Root? | Superuser with UID 0 |
| What is PID? | Process ID |
| What is Process? | Running program |
| What is SSH? | Secure remote login protocol |
| What is LVM? | Flexible logical storage management |
| What is Swap? | Virtual memory on disk |

---


# 26. How do you check disk usage in Linux?

### Answer

Use:

```bash
df -h
```

To check the size of a specific directory:

```bash
du -sh /var/log
```

---

# 27. What is the difference between `df` and `du`?

### Answer

| df | du |
|----|----|
| Shows filesystem usage | Shows directory/file usage |
| Entire disk | Specific files/directories |

---

# 28. How do you check memory usage?

### Answer

```bash
free -h
```

For real-time monitoring:

```bash
top
```

or

```bash
htop
```

---

# 29. How do you check CPU usage?

### Answer

```bash
top
```

or

```bash
htop
```

Other useful commands:

```bash
mpstat

vmstat
```

---

# 30. How do you list running processes?

### Answer

```bash
ps -ef
```

or

```bash
ps aux
```

---

# 31. What is the difference between `kill` and `kill -9`?

### Answer

`kill`

- Sends **SIGTERM (15)**
- Allows graceful shutdown.

`kill -9`

- Sends **SIGKILL (9)**
- Immediately terminates the process without cleanup.

---

# 32. How do you find a process by name?

### Answer

```bash
pgrep nginx
```

or

```bash
ps -ef | grep nginx
```

---

# 33. What is the difference between Foreground and Background processes?

### Answer

Foreground

- Runs in the current terminal.

Background

- Runs independently, allowing the terminal to remain available.

---

# 34. What is `systemd`?

### Answer

`systemd` is the default Linux init system and service manager responsible for starting, stopping, monitoring, and managing system services.

PID:

```text
1
```

---

# 35. How do you check the status of a service?

### Answer

```bash
systemctl status nginx
```

---

# 36. How do you restart a service?

### Answer

```bash
sudo systemctl restart nginx
```

---

# 37. What is the purpose of `/etc/fstab`?

### Answer

It stores the configuration for filesystems that should be mounted automatically during system startup.

---

# 38. How do you view mounted filesystems?

### Answer

```bash
findmnt
```

or

```bash
mount
```

---

# 39. What command displays IP addresses?

### Answer

```bash
ip addr
```

or

```bash
hostname -I
```

---

# 40. How do you verify network connectivity?

### Answer

```bash
ping google.com
```

---

# 41. How do you troubleshoot DNS issues?

### Answer

Use:

```bash
nslookup example.com
```

or

```bash
dig example.com
```

Verify:

- DNS resolution
- `/etc/resolv.conf`
- Network connectivity

---

# 42. What is the difference between `curl` and `wget`?

### Answer

`curl`

- Tests APIs
- Sends HTTP requests
- Transfers data

`wget`

- Downloads files
- Supports download resume

---

# 43. What is the difference between `scp` and `rsync`?

### Answer

`scp`

- Secure file copy
- Copies the entire file

`rsync`

- Synchronizes files
- Transfers only changed data
- Faster for repeated transfers

---

# 44. How do you display listening ports?

### Answer

```bash
ss -tuln
```

Older systems may also use:

```bash
netstat -tuln
```

---

# 45. What is Shell Scripting?

### Answer

Shell scripting is writing Linux commands in a file so they can be executed automatically for automation and administration.

---

# 46. What is the purpose of `chmod`?

### Answer

`chmod` changes file and directory permissions.

Example:

```bash
chmod 755 script.sh
```

---

# 47. What is the difference between Authentication and Authorization?

### Answer

Authentication

- Verifies identity.

Authorization

- Determines permissions after authentication.

---

# 48. What is SELinux?

### Answer

SELinux is a Mandatory Access Control (MAC) framework that enforces security policies beyond standard Linux permissions.

---

# 49. What is the purpose of a Firewall?

### Answer

A firewall filters incoming and outgoing network traffic based on configured security rules.

---

# 50. What are the first steps when troubleshooting a Linux server?

### Answer

A structured approach is:

1. Check CPU usage (`top`)
2. Check memory (`free -h`)
3. Check disk usage (`df -h`)
4. Verify running processes (`ps -ef`)
5. Check services (`systemctl status`)
6. Review logs (`journalctl`)
7. Test networking (`ping`, `curl`, `ss`)

---

# 📌 Quick Revision

| Question | One-Line Answer |
|----------|-----------------|
| `df` vs `du` | Filesystem usage vs directory usage |
| `free -h` | Memory usage |
| `top` | CPU and process monitoring |
| `ps -ef` | List running processes |
| `kill -9` | Forcefully terminates a process |
| `systemd` | Linux init system and service manager |
| `systemctl` | Manage services |
| `/etc/fstab` | Automatic filesystem mounts |
| `ip addr` | Display IP addresses |
| `ping` | Test network connectivity |
| `curl` | Test APIs and HTTP services |
| `scp` | Secure file copy |
| `rsync` | Efficient file synchronization |
| `ss` | Display sockets and listening ports |
| `SELinux` | Mandatory Access Control |

---


# 51. What happens when you type a Linux command?

### Answer

When a command is executed:

```text
User

↓

Shell (Bash)

↓

Kernel

↓

System Call

↓

CPU & Memory

↓

Command Output
```

The shell interprets the command, the kernel executes it using system calls, and the result is returned to the user.

---

# 52. Explain the Linux Boot Process.

### Answer

The Linux boot process follows these stages:

```text
BIOS / UEFI

↓

Bootloader (GRUB)

↓

Kernel

↓

initramfs

↓

systemd (PID 1)

↓

Services

↓

Login Prompt
```

---

# 53. What is an inode?

### Answer

An inode is a data structure that stores file metadata such as:

- Owner
- Permissions
- Size
- Timestamps
- Disk block locations

It does **not** store the filename.

---

# 54. What causes "No space left on device" when disk space is available?

### Answer

Possible reasons:

- Inodes exhausted
- Read-only filesystem
- Quota limits

Check inode usage:

```bash
df -i
```

---

# 55. Explain the difference between Hard Links and Symbolic Links.

### Answer

Hard Link

- Shares the same inode
- Cannot span filesystems
- Remains valid if the original filename is deleted

Symbolic Link

- Has its own inode
- Points to a file path
- Can span filesystems
- Breaks if the target is removed

---

# 56. How do you troubleshoot a server with high CPU usage?

### Answer

Typical steps:

1. Run:

```bash
top
```

2. Identify the process.

3. Check logs.

4. Verify application health.

5. Restart or optimize the application if required.

---

# 57. How do you troubleshoot high memory usage?

### Answer

Commands:

```bash
free -h

top

htop
```

Identify:

- Memory leaks
- Cache usage
- High-memory processes

---

# 58. How do you troubleshoot disk issues?

### Answer

Commands:

```bash
df -h

du -sh

df -i

lsblk

findmnt
```

Check:

- Free space
- Inode usage
- Mount status

---

# 59. How do you troubleshoot network issues?

### Answer

Use:

```bash
ip addr

ping

traceroute

nslookup

curl

ss

tcpdump
```

Check:

- IP configuration
- DNS
- Routing
- Firewall
- Listening services

---

# 60. How do you troubleshoot a service that won't start?

### Answer

Check:

```bash
systemctl status service-name

journalctl -u service-name
```

Verify:

- Configuration
- Dependencies
- Permissions
- Port conflicts

---

# 61. Explain the Principle of Least Privilege (PoLP).

### Answer

Users and applications should receive **only the permissions required** to perform their tasks.

This minimizes the impact of accidental mistakes and security vulnerabilities.

---

# 62. Why should applications not run as root?

### Answer

Running applications as root increases security risks because a compromised application would have unrestricted system access.

Use dedicated service accounts whenever possible.

---

# 63. How do you secure SSH?

### Answer

Best practices:

- SSH keys
- Disable root login
- Disable password authentication (where practical)
- Enable MFA
- Restrict access with a firewall

---

# 64. What is LVM and why is it used?

### Answer

LVM provides flexible storage management.

Advantages:

- Resize logical volumes
- Easier storage expansion
- Better storage administration

---

# 65. Why is RAID not considered a backup?

### Answer

RAID protects against disk failure but **does not protect against**:

- Accidental deletion
- File corruption
- Malware or ransomware
- Human error

Backups are still required.

---

# 66. How do you monitor Linux systems?

### Answer

Common tools:

- top
- htop
- vmstat
- iostat
- Prometheus
- Grafana
- ELK Stack

---

# 67. What logs do you check during troubleshooting?

### Answer

Common logs:

```text
/var/log/syslog

/var/log/auth.log

/var/log/messages

journalctl
```

(RHEL-based systems commonly use `/var/log/secure` for authentication logs.)

---

# 68. Explain the Linux permission model.

### Answer

Permissions apply to:

- Owner
- Group
- Others

Permission types:

- Read
- Write
- Execute

Special permissions:

- SUID
- SGID
- Sticky Bit

---

# 69. What is SELinux?

### Answer

SELinux is a Mandatory Access Control framework that enforces security policies independently of traditional Linux permissions.

---

# 70. Explain systemd.

### Answer

systemd is the Linux initialization and service management system.

Responsibilities include:

- Starting services
- Managing dependencies
- Handling boot
- Monitoring services

---

# 71. Scenario: Website is Down

### Question

Users cannot access the website.

How would you troubleshoot?

### Answer

1. Check service

```bash
systemctl status nginx
```

2. Check port

```bash
ss -tuln
```

3. Check logs

```bash
journalctl -u nginx
```

4. Check disk

```bash
df -h
```

5. Test locally

```bash
curl localhost
```

6. Verify firewall

---

# 72. Scenario: Server is Slow

### Answer

Check:

- CPU

```bash
top
```

- Memory

```bash
free -h
```

- Disk

```bash
df -h
```

- Processes

```bash
ps -ef
```

- Logs

```bash
journalctl
```

---

# 73. Scenario: SSH Login Fails

### Answer

Verify:

- SSH service

```bash
systemctl status sshd
```

- Firewall
- Network
- SSH keys
- Authentication logs

```bash
journalctl -u sshd
```

---

# 74. Scenario: Filesystem Suddenly Becomes Read-Only

### Answer

Possible causes:

- Disk errors
- Filesystem corruption
- Hardware failures

Check:

```bash
dmesg

mount

journalctl
```

---

# 75. Why is Linux the preferred operating system for DevOps?

### Answer

Because Linux provides:

- Stability
- Automation
- Shell Scripting
- Cloud Compatibility
- Kubernetes Support
- Security
- Open Source Ecosystem

Most production cloud infrastructure runs Linux.

---

# 📌 Final Linux Interview Cheat Sheet

### Process Commands

```bash
ps

top

htop

kill

pgrep
```

---

### Storage Commands

```bash
df -h

du -sh

lsblk

findmnt
```

---

### Networking Commands

```bash
ip addr

ping

curl

ssh

ss

tcpdump
```

---

### Service Commands

```bash
systemctl

journalctl
```

---

### Security Commands

```bash
chmod

chown

getenforce

ufw

firewall-cmd
```

---

# 🎯 Top 10 Linux Interview Questions

1. What is Linux?
2. What is the Kernel?
3. Explain the Linux Boot Process.
4. Difference between Hard Link and Symbolic Link.
5. Explain Linux Permissions.
6. Difference between `kill` and `kill -9`.
7. How do you troubleshoot high CPU usage?
8. How do you troubleshoot a server that is down?
9. Explain LVM.
10. How do you secure a Linux server?

---

# 💡 Expert Interview Tips

### Before the Interview

- Revise Linux fundamentals.
- Practice common commands.
- Understand system architecture.
- Learn troubleshooting workflows.

---

### During the Interview

- Explain your thought process.
- Mention the commands you would use.
- Describe why you chose those commands.
- Relate answers to production environments.

---

### Example Response Structure

```text
Problem

↓

Commands Used

↓

Analysis

↓

Root Cause

↓

Solution

↓

Verification
```

Interviewers often value a structured troubleshooting approach more than simply memorizing commands.

---

# 🎉 Linux Learning Path Completed

Congratulations! You have completed the entire **Linux** module of the **Engineer Resources** roadmap.

You now have comprehensive coverage of:

- ✅ Linux Fundamentals
- ✅ Filesystem
- ✅ Permissions
- ✅ Process Management
- ✅ Networking
- ✅ Storage
- ✅ Shell Scripting
- ✅ Security
- ✅ Interview Preparation (75 Questions)

This provides a solid foundation for Linux Administration, DevOps Engineering, Cloud Engineering, Kubernetes, Platform Engineering, and Site Reliability Engineering (SRE).

---

# 🚀 What's Next?

➡️ **Git/**

Start with:

**`01-git-fundamentals.md`**

Topics include:

- Git Architecture
- Repositories
- Commits
- Branches
- Merging
- Git Workflow
- Collaboration
- Git Best Practices
- Git Interview Questions