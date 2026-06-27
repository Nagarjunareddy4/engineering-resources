# 💾 Linux Storage

This guide explains everything you need to know about Linux Storage.

If you've ever wondered:

- What is Linux Storage?
- What is the difference between a Disk and a Partition?
- What is a File System?
- How does Linux access storage?
- What are Mount Points?
- Why is Linux Storage important?

This guide is for you.

---

# 📑 Table of Contents

1. What is Linux Storage?
2. Storage Architecture
3. Disks
4. Partitions
5. File Systems
6. Common Interview Questions

---

# 1. What is Linux Storage?

## Answer

**Linux Storage** refers to how Linux manages, organizes, and accesses physical and virtual storage devices.

Storage includes:

- Hard Disk Drives (HDD)
- Solid State Drives (SSD)
- NVMe Drives
- USB Drives
- SAN Storage
- NAS Storage
- Cloud Volumes

Linux provides a flexible storage model where devices are attached to the filesystem through mount points.

---

## Storage Architecture

```text
Application

↓

File System

↓

Logical Storage

↓

Physical Disk

↓

Hardware
```

---

## Key Characteristics

Linux Storage provides:

- High Performance
- Scalability
- Flexibility
- Reliability
- Data Protection

---

## Real World Example

An Azure Virtual Machine uses a managed disk attached as:

```text
/dev/sdc
```

Linux formats it with **ext4**, mounts it at:

```text
/data
```

and applications store their data there.

---

## Benefits

Linux Storage provides:

- Efficient Data Management
- Reliable Storage
- Flexible Expansion
- Secure Access
- Enterprise Scalability

---

## Production Tip

Separate operating system storage from application and database storage to simplify maintenance, backups, and disaster recovery.

---

## Interview Question

### Question

What is Linux Storage?

### Answer

Linux Storage is the system used to manage disks, partitions, filesystems, and mounted storage devices for storing and accessing data.

---

# 2. Linux Storage Architecture

## Answer

Linux follows a layered storage architecture.

Applications never communicate directly with physical disks.

Instead, requests pass through multiple storage layers.

---

## Architecture

```text
Applications

↓

File System

↓

Logical Volume (Optional)

↓

Partition

↓

Physical Disk

↓

Storage Hardware
```

---

## Storage Layers

| Layer | Purpose |
|--------|---------|
| Application | Reads/Writes files |
| File System | Organizes files |
| Logical Volume | Flexible storage management |
| Partition | Divides physical disks |
| Physical Disk | Stores data permanently |

---

## Production Example

A MySQL database stores its data on an LVM logical volume formatted with the XFS filesystem.

---

## Production Tip

Understanding storage layers makes troubleshooting disk issues much easier.

---

## Interview Question

### Question

What are the layers of Linux Storage?

### Answer

Applications → File System → Logical Volume (optional) → Partition → Physical Disk.

---

# 3. Disks

## Answer

A **Disk** is a physical or virtual storage device used to store data.

Linux identifies disks using device files under:

```text
/dev
```

---

## Common Disk Names

| Device | Description |
|---------|-------------|
| /dev/sda | First SCSI/SATA disk |
| /dev/sdb | Second SCSI/SATA disk |
| /dev/nvme0n1 | NVMe SSD |
| /dev/vda | Virtual disk (KVM/Cloud) |

---

## View Available Disks

```bash
lsblk
```

---

Example

```text
NAME

sda

sdb

nvme0n1
```

---

## Architecture

```text
Physical Disk

↓

Linux Device

↓

/dev/sda
```

---

## Production Example

A production server has:

```text
/dev/sda

Operating System

/dev/sdb

Application Data

/dev/sdc

Database Storage
```

---

## Production Tip

Use dedicated disks for databases and application data to improve performance and simplify storage management.

---

## Interview Question

### Question

How does Linux identify storage disks?

### Answer

Linux represents storage devices as files under the `/dev` directory, such as `/dev/sda` or `/dev/nvme0n1`.

---

# 4. Partitions

## Answer

A **Partition** divides a physical disk into multiple logical sections.

Each partition can have:

- Its own filesystem
- Different mount point
- Separate purpose

---

## Example

```text
Disk

/dev/sda

↓

Partition 1

/dev/sda1

↓

Partition 2

/dev/sda2
```

---

## Architecture

```text
Physical Disk

↓

Partition Table

↓

Partitions

↓

File Systems
```

---

## View Partitions

```bash
lsblk
```

or

```bash
sudo fdisk -l
```

---

## Production Example

A production server may use:

```text
/dev/sda1

/

Operating System

/dev/sda2

/var

Application Logs

/dev/sdb1

/data

Database Files
```

---

## Production Tip

Separate critical workloads into different partitions to improve maintenance and reduce the impact of storage issues.

---

## Interview Question

### Question

What is a partition?

### Answer

A partition is a logical division of a physical disk that can contain its own filesystem and mount point.

---

# 5. File Systems

## Answer

A **File System** defines how data is stored, organized, and retrieved on a storage device.

Without a filesystem, Linux cannot store files on a disk.

---

## Common Linux File Systems

| File System | Description |
|--------------|-------------|
| ext4 | Default for many Linux distributions |
| XFS | Enterprise-grade, high performance |
| Btrfs | Advanced filesystem with snapshots |
| FAT32 | Cross-platform compatibility |
| exFAT | Large removable storage |
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

## Architecture

```text
Application

↓

File System

↓

Disk Blocks

↓

Storage Device
```

---

## Production Example

An Ubuntu server stores application files using the **ext4** filesystem, while a RHEL database server uses **XFS** for improved scalability.

---

## Production Tip

Choose a filesystem based on workload requirements. ext4 is ideal for general-purpose use, while XFS is preferred for many enterprise workloads.

---

## Interview Question

### Question

What is a filesystem?

### Answer

A filesystem is the method used by an operating system to organize, store, and retrieve files on a storage device.

---

## 📌 Quick Revision

| Topic | Key Concept |
|--------|-------------|
| Linux Storage | Management of disks and data |
| Storage Architecture | Application → Filesystem → Partition → Disk |
| Disk | Physical or virtual storage device |
| Partition | Logical division of a disk |
| Filesystem | Organizes and stores files |
| `lsblk` | Displays disks and partitions |
| `fdisk -l` | Lists disk partition information |
| `df -T` | Displays mounted filesystems and types |

---

# 6. Mounting & Unmounting Filesystems

## Answer

In Linux, storage devices are accessed by **mounting** them to a directory called a **mount point**.

Unlike Windows, Linux does not use drive letters such as `C:` or `D:`. Instead, every storage device becomes part of the single directory hierarchy.

---

## Mounting Workflow

```text
Storage Device

↓

Filesystem

↓

Mount Point

↓

Accessible Files
```

---

## Mount a Filesystem

```bash
sudo mount /dev/sdb1 /data
```

---

## Verify Mounted Filesystems

```bash
mount
```

or

```bash
findmnt
```

---

## Unmount a Filesystem

```bash
sudo umount /data
```

---

## View Mounted Filesystems

```bash
df -h
```

---

## Production Example

A new Azure Managed Disk is attached to a Linux VM, formatted with **ext4**, and mounted at:

```text
/data
```

to store application files.

---

## Production Tip

Always unmount a filesystem before removing or detaching a storage device to prevent data corruption.

---

## Interview Question

### Question

What is mounting in Linux?

### Answer

Mounting is the process of attaching a filesystem to a directory so its contents become accessible through the Linux directory hierarchy.

---

# 7. /etc/fstab

## Answer

The **`/etc/fstab`** file defines which filesystems should be mounted automatically during system boot.

Without an entry in `fstab`, manually mounted filesystems will not persist after a reboot.

---

## View fstab

```bash
cat /etc/fstab
```

---

## Example Entry

```text
UUID=8a2d-45f3   /data   ext4   defaults   0   2
```

---

## Fields Explained

| Field | Description |
|--------|-------------|
| Device / UUID | Storage device |
| Mount Point | Directory where it is mounted |
| Filesystem | ext4, XFS, etc. |
| Options | Mount options |
| Dump | Backup option |
| Pass | Filesystem check order |

---

## Test fstab

After editing:

```bash
sudo mount -a
```

This validates the configuration without rebooting.

---

## Production Example

Database storage is permanently mounted at:

```text
/var/lib/mysql
```

using an entry in `/etc/fstab`.

---

## Production Tip

Use **UUIDs** instead of device names like `/dev/sdb1`, because device names can change after reboots or hardware changes.

---

## Interview Question

### Question

What is the purpose of `/etc/fstab`?

### Answer

`/etc/fstab` defines filesystems that should be mounted automatically when the system boots.

---

# 8. Logical Volume Manager (LVM)

## Answer

**LVM (Logical Volume Manager)** provides flexible storage management by abstracting physical storage into logical volumes.

Unlike traditional partitions, LVM allows administrators to resize storage more easily.

---

## LVM Architecture

```text
Physical Disk

↓

Physical Volume (PV)

↓

Volume Group (VG)

↓

Logical Volume (LV)

↓

Filesystem

↓

Mount Point
```

---

## LVM Components

| Component | Purpose |
|-----------|---------|
| PV | Physical Volume |
| VG | Volume Group |
| LV | Logical Volume |

---

## Display LVM Information

Physical Volumes

```bash
pvs
```

Volume Groups

```bash
vgs
```

Logical Volumes

```bash
lvs
```

---

## Production Example

A database server uses LVM so storage can be expanded without reinstalling the operating system or recreating partitions.

---

## Production Tip

Use LVM for production servers where future storage expansion is expected.

---

## Interview Question

### Question

What is LVM?

### Answer

LVM is a storage management framework that provides flexible logical volumes on top of physical storage devices.

---

# 9. Swap Space

## Answer

**Swap Space** is disk space used as virtual memory when physical RAM becomes full.

Swap is slower than RAM but helps prevent applications from crashing due to memory exhaustion.

---

## Memory Workflow

```text
Application

↓

RAM Full

↓

Swap Space

↓

Disk
```

---

## View Swap

```bash
swapon --show
```

or

```bash
free -h
```

---

## Enable Swap

```bash
sudo swapon /swapfile
```

---

## Disable Swap

```bash
sudo swapoff /swapfile
```

---

## Production Example

A Linux VM with limited memory uses a swap file to handle temporary memory spikes during software updates.

---

## Production Tip

Swap should not replace physical RAM. Excessive swap usage often indicates that additional memory is required.

---

## Interview Question

### Question

What is Swap Space?

### Answer

Swap Space is disk space used as virtual memory when physical RAM is exhausted.

---

# 10. RAID (Redundant Array of Independent Disks)

## Answer

**RAID** combines multiple physical disks to improve:

- Performance
- Reliability
- Fault Tolerance

---

## Common RAID Levels

| RAID | Description |
|------|-------------|
| RAID 0 | Striping for performance (no redundancy) |
| RAID 1 | Mirroring for redundancy |
| RAID 5 | Striping with distributed parity |
| RAID 6 | Double parity for higher fault tolerance |
| RAID 10 | Mirroring + Striping |

---

## RAID Overview

```text
Application

↓

Filesystem

↓

RAID

↓

Multiple Disks
```

---

## Production Example

A production database server stores data on **RAID 10** to achieve both high performance and redundancy.

---

## Production Tip

Choose the RAID level based on workload requirements. RAID improves availability but is **not** a replacement for backups.

---

## Interview Question

### Question

What is RAID?

### Answer

RAID is a technology that combines multiple disks to improve performance, redundancy, or both.

---

# 11. Essential Storage Commands

## Answer

Linux provides several commands to inspect storage devices, filesystems, and disk usage.

---

## Check Filesystem Usage

```bash
df -h
```

---

## Check Directory Size

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

## List Disk Partitions

```bash
sudo fdisk -l
```

---

## View Mounted Filesystems

```bash
findmnt
```

---

## Workflow

```text
Storage Device

↓

Linux Commands

↓

Storage Information

↓

Administrator
```

---

## Production Example

Before deploying a new application, a DevOps engineer runs:

```bash
df -h
```

to ensure sufficient disk space is available.

---

## Production Tip

Regularly monitor storage usage and filesystem health to prevent outages caused by full disks.

---

## Interview Question

### Question

Which Linux command displays available disk space?

### Answer

The `df -h` command displays filesystem disk usage in a human-readable format.

---

## 📌 Quick Revision

| Command | Purpose |
|----------|---------|
| `mount` | Mount a filesystem |
| `umount` | Unmount a filesystem |
| `findmnt` | Display mounted filesystems |
| `cat /etc/fstab` | View automatic mount configuration |
| `pvs` | Display Physical Volumes |
| `vgs` | Display Volume Groups |
| `lvs` | Display Logical Volumes |
| `swapon --show` | Display active swap |
| `df -h` | Display disk usage |
| `du -sh` | Display directory size |
| `lsblk` | Display disks and partitions |
| `blkid` | Display filesystem UUIDs |
| `fdisk -l` | List disk partitions |

---

# 12. Storage Troubleshooting

## Answer

Storage issues are among the most common causes of production outages.

Effective troubleshooting involves identifying problems with:

- Disk Space
- Mount Points
- File Systems
- Partitions
- LVM
- Hardware
- Permissions

---

## Troubleshooting Workflow

```text
Problem Reported

↓

Check Disk Usage

↓

Verify Mount Points

↓

Check Filesystem

↓

Review System Logs

↓

Resolve Issue

↓

Verify Recovery
```

---

## Useful Commands

Check disk usage

```bash
df -h
```

Check inode usage

```bash
df -i
```

Check mounted filesystems

```bash
findmnt
```

List storage devices

```bash
lsblk
```

Check filesystem UUID

```bash
blkid
```

View kernel messages

```bash
dmesg
```

View system logs

```bash
journalctl
```

---

## Production Example

An application cannot write files.

The administrator:

1. Checks disk usage using `df -h`
2. Confirms the filesystem is mounted
3. Verifies file permissions
4. Reviews logs using `journalctl`

The issue is traced to a full filesystem.

---

## Production Tip

Always investigate the root cause before expanding storage. The issue may simply be unnecessary files or log growth.

---

## Interview Question

### Question

How do you troubleshoot Linux storage issues?

### Answer

Check disk usage, verify mount points, inspect filesystems, review logs, and confirm permissions before making storage changes.

---

# 13. Common Storage Problems

## Answer

Storage-related issues can affect application availability and system performance.

---

## Problem 1

### Disk Full

Symptoms

- Unable to create files
- Applications fail
- Database write errors

Check

```bash
df -h
```

---

## Problem 2

### Inode Exhaustion

Symptoms

- Free disk space exists
- New files cannot be created

Check

```bash
df -i
```

---

## Problem 3

### Filesystem Not Mounted

Verify

```bash
findmnt
```

or

```bash
mount
```

---

## Problem 4

### Read-Only Filesystem

Symptoms

Applications cannot write data.

Check

```bash
mount
```

---

## Problem 5

### Incorrect `/etc/fstab`

A misconfigured `fstab` file can prevent the system from mounting filesystems correctly during boot.

Validate changes with:

```bash
sudo mount -a
```

before rebooting.

---

## Production Example

A server fails to boot after an incorrect `/etc/fstab` entry. The administrator starts the system in rescue mode, fixes the configuration, validates it with `mount -a`, and reboots successfully.

---

## Production Tip

Test storage configuration changes in a maintenance window or non-production environment before applying them to production systems.

---

## Interview Question

### Question

What are common Linux storage problems?

### Answer

Common problems include full disks, inode exhaustion, missing mount points, read-only filesystems, and incorrect `/etc/fstab` configurations.

---

# 14. Common Beginner Mistakes

## Mistake 1

❌ Editing `/etc/fstab` without testing.

Always validate:

```bash
sudo mount -a
```

before rebooting.

---

## Mistake 2

❌ Removing mounted storage devices.

Always unmount first:

```bash
sudo umount /data
```

---

## Mistake 3

❌ Ignoring disk usage.

Regularly monitor:

```bash
df -h
```

---

## Mistake 4

❌ Storing everything on the root filesystem.

Separate:

- Application Data
- Database Files
- Logs
- Backups

onto dedicated storage where appropriate.

---

## Mistake 5

❌ Assuming RAID is a backup.

RAID provides redundancy, **not** backup protection.

---

## Production Tip

Follow the **3-2-1 Backup Rule**:

- 3 copies of your data
- 2 different storage media
- 1 offsite copy

---

## Interview Question

### Question

What are common Linux storage mistakes?

### Answer

Common mistakes include editing `fstab` without testing, removing mounted disks, ignoring storage monitoring, storing everything on the root filesystem, and treating RAID as a backup.

---

# 15. Linux Storage Best Practices

## Answer

Following storage best practices improves reliability, scalability, and disaster recovery.

---

## Disk Organization

✅ Separate operating system, application, database, and backup storage.

---

## Monitoring

✅ Monitor:

- Disk Usage
- Inode Usage
- Filesystem Health
- Storage Performance

---

## Filesystems

✅ Choose the appropriate filesystem for the workload.

---

## LVM

✅ Use LVM where storage expansion is expected.

---

## RAID

✅ Use RAID for fault tolerance where appropriate.

---

## Backups

✅ Perform regular backups and periodically test restoration procedures.

---

## Documentation

✅ Document:

- Storage Layout
- Mount Points
- Filesystem Types
- Backup Procedures

---

## Production Example

A production environment:

- Uses LVM for database storage.
- Stores logs on a dedicated partition.
- Uses RAID 10 for performance and redundancy.
- Performs daily backups to cloud storage.
- Monitors storage metrics with Prometheus and Grafana.

---

## Production Tip

Implement storage monitoring with alerting so administrators are notified before disks become critically full.

---

## Interview Question

### Question

What are Linux storage best practices?

### Answer

Use separate storage for critical workloads, monitor capacity, choose suitable filesystems, use LVM for flexibility, perform backups, and document the storage configuration.

---

# 16. Backup & Recovery Concepts

## Answer

Backups protect data from accidental deletion, corruption, hardware failures, and disasters.

A recovery plan is only effective if restoration procedures are tested regularly.

---

## Backup Workflow

```text
Production Data

↓

Scheduled Backup

↓

Backup Storage

↓

Recovery Testing

↓

Disaster Recovery
```

---

## Backup Types

| Type | Description |
|------|-------------|
| Full | Complete backup of all selected data |
| Incremental | Backs up only changes since the last backup |
| Differential | Backs up changes since the last full backup |

---

## Production Example

A company performs:

- Weekly Full Backup
- Daily Incremental Backups
- Monthly Restore Tests

to ensure recoverability.

---

## Production Tip

A backup that has never been restored and verified should not be considered reliable.

---

## Interview Question

### Question

Why is backup verification important?

### Answer

Backup verification ensures that backed-up data can be successfully restored when needed.

---

# 📌 Quick Revision

### Linux Storage Architecture

```text
Application

↓

Filesystem

↓

Logical Volume (Optional)

↓

Partition

↓

Disk
```

---

### Storage Workflow

```text
Disk

↓

Partition

↓

Filesystem

↓

Mount Point

↓

Application
```

---

### Important Commands

```bash
lsblk

fdisk -l

df -h

df -i

du -sh

blkid

mount

umount

findmnt

pvs

vgs

lvs

swapon --show

journalctl

dmesg
```

---

# Summary

After completing this guide, you should understand:

✅ Linux Storage Fundamentals

✅ Storage Architecture

✅ Disks

✅ Partitions

✅ Filesystems

✅ Mounting & Unmounting

✅ `/etc/fstab`

✅ Logical Volume Manager (LVM)

✅ Swap Space

✅ RAID

✅ Storage Commands

✅ Storage Troubleshooting

✅ Linux Storage Best Practices

✅ Backup & Recovery Concepts

---

# Interview Quick Revision

| Question | One-Line Answer |
|----------|-----------------|
| What is Linux Storage? | The system used to manage disks, partitions, filesystems, and mounted storage |
| What is a Partition? | A logical division of a physical disk |
| What is a Filesystem? | A structure used to organize and store files |
| What is Mounting? | Attaching a filesystem to a directory in the Linux hierarchy |
| What is `/etc/fstab`? | A configuration file for automatic filesystem mounting during boot |
| What is LVM? | A storage management framework providing flexible logical volumes |
| What is Swap Space? | Disk space used as virtual memory when RAM is full |
| What is RAID? | A technology that combines multiple disks for performance and/or redundancy |
| How do you check disk usage? | `df -h` |
| Why is RAID not a backup? | RAID provides redundancy but does not protect against accidental deletion, corruption, or disasters |

---

# 🎉 Linux Storage Module Completed

Congratulations! You have completed **Linux Storage**, including:

- ✅ Linux Storage Fundamentals
- ✅ Storage Architecture
- ✅ Disks
- ✅ Partitions
- ✅ Filesystems
- ✅ Mounting & Unmounting
- ✅ `/etc/fstab`
- ✅ Logical Volume Manager (LVM)
- ✅ Swap Space
- ✅ RAID
- ✅ Essential Storage Commands
- ✅ Storage Troubleshooting
- ✅ Linux Storage Best Practices
- ✅ Backup & Recovery Concepts

You now have a solid understanding of Linux storage management, disk administration, and storage troubleshooting—essential skills for Linux Administration, DevOps, Cloud Engineering, and Site Reliability Engineering (SRE).

---

# 🚀 What's Next?

➡️ **07-shell-scripting.md**

In the next guide, you'll learn:

- What is Shell Scripting?
- Bash Fundamentals
- Variables
- Operators
- Conditional Statements
- Loops
- Functions
- Arrays
- File Handling
- Automation Scripts
- Production Best Practices
- Common Interview Questions