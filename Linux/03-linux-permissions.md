# 🐧 Linux Permissions

This guide explains everything you need to know about Linux Users, Groups, Ownership, and File Permissions.

If you've ever wondered:

- How does Linux control file access?
- What are Users and Groups?
- What is file ownership?
- What do `rwx` permissions mean?
- Why are Linux permissions important?

This guide is for you.

---

# 📑 Table of Contents

1. What are Linux Permissions?
2. Linux Users
3. Linux Groups
4. File Ownership
5. Understanding File Permissions
6. Common Interview Questions

---

# 1. What are Linux Permissions?

## Answer

Linux Permissions are the security mechanism that controls **who can access files and directories and what actions they can perform**.

Every file and directory in Linux has permissions assigned to it.

Permissions determine whether a user can:

- Read a file
- Modify a file
- Execute a file
- Access a directory

This permission model is one of the main reasons Linux is considered highly secure.

---

## Permission Architecture

```text
User

↓

Permission Check

↓

File / Directory

↓

Allow or Deny Access
```

---

## Key Characteristics

Linux permissions provide:

- Security
- Access Control
- Multi-user Isolation
- Protection of System Files
- Controlled Collaboration

---

## Real World Example

A company's application configuration file is owned by the **root** user.

Regular users can read the configuration but cannot modify it, preventing accidental or unauthorized changes.

---

## Benefits

Linux Permissions provide:

- Secure File Access
- Data Protection
- User Isolation
- Controlled Administration
- Compliance with Security Policies

---

## Production Tip

Follow the **Principle of Least Privilege** by granting users only the permissions they need to perform their tasks.

---

## Interview Question

### Question

What are Linux Permissions?

### Answer

Linux Permissions define which users can read, write, or execute files and directories, providing secure access control in a multi-user operating system.

---

# 2. Linux Users

## Answer

A **User** represents an individual or service that interacts with the Linux system.

Each user has:

- Username
- User ID (UID)
- Home Directory
- Default Shell

Linux supports multiple users working on the same system simultaneously.

---

## Types of Users

### Root User

- Username: `root`
- UID: `0`
- Full administrative privileges

---

### Regular User

Created for daily work.

Example:

```text
john

ubuntu

developer
```

---

### System User

Used by services and applications.

Examples:

```text
www-data

mysql

nginx

postgres
```

---

## User Architecture

```text
Linux System

↓

Root User

Regular Users

System Users
```

---

## View Current User

```bash
whoami
```

---

## View User ID

```bash
id
```

Example Output

```text
uid=1000(john)

gid=1000(john)

groups=1000(john),27(sudo)
```

---

## Production Example

A web server runs using the `www-data` user instead of the root user to reduce security risks.

---

## Production Tip

Avoid using the root account for everyday administration. Use a regular account with `sudo` privileges instead.

---

## Interview Question

### Question

What are the different types of Linux users?

### Answer

Linux has three main user types: the root user, regular users, and system users used by services and applications.

---

# 3. Linux Groups

## Answer

A **Group** is a collection of users who share common permissions.

Groups simplify permission management by allowing administrators to assign permissions to a group instead of configuring each user individually.

---

## Example

Developers:

```text
alice

bob

charlie
```

All belong to:

```text
developers
```

---

## Architecture

```text
Users

↓

Group

↓

Shared Permissions
```

---

## View Groups

Current user's groups:

```bash
groups
```

View all groups:

```bash
cat /etc/group
```

---

## Benefits

Groups provide:

- Easier Administration
- Shared Access
- Centralized Permission Management
- Reduced Configuration Effort

---

## Production Example

All DevOps engineers belong to the `devops` group, allowing them to manage deployment scripts without granting unnecessary permissions to other users.

---

## Production Tip

Assign permissions to groups whenever possible instead of individual users. This makes user management much easier as teams grow.

---

## Interview Question

### Question

Why are Groups used in Linux?

### Answer

Groups allow multiple users to share common permissions, simplifying access control and administration.

---

# 4. File Ownership

## Answer

Every file and directory in Linux has an **Owner** and a **Group**.

Ownership determines who controls the file and which group members may have additional access.

---

## View Ownership

```bash
ls -l
```

Example Output

```text
-rw-r--r-- 1 john developers 1200 file.txt
```

---

## Breakdown

```text
Owner:

john

Group:

developers
```

---

## Ownership Architecture

```text
File

↓

Owner

↓

Group

↓

Other Users
```

---

## Production Example

An application log file is owned by:

```text
Owner : nginx

Group : nginx
```

Only the application and authorized administrators can modify it.

---

## Production Tip

Always verify file ownership after deploying applications or restoring backups to prevent permission-related issues.

---

## Interview Question

### Question

What is file ownership in Linux?

### Answer

File ownership identifies the user and group responsible for a file or directory and determines who can manage its permissions.

---

# 5. Understanding File Permissions

## Answer

Linux permissions define what actions can be performed on a file or directory.

There are **three permission types**:

| Symbol | Permission | Meaning |
|--------|------------|---------|
| `r` | Read | View file contents or list directory contents |
| `w` | Write | Modify a file or create/delete items in a directory (subject to directory permissions) |
| `x` | Execute | Run a file as a program or access a directory |

---

## Permission Categories

Permissions are assigned to three categories:

- Owner (User)
- Group
- Others

---

## Example

```text
-rwxr-xr--
```

Breakdown:

```text
-

Regular File

rwx

Owner

r-x

Group

r--

Others
```

---

## Permission Architecture

```text
File

↓

Owner

↓

Group

↓

Others
```

---

## Numeric Representation

| Number | Permission |
|---------|------------|
| 7 | rwx |
| 6 | rw- |
| 5 | r-x |
| 4 | r-- |
| 3 | -wx |
| 2 | -w- |
| 1 | --x |
| 0 | --- |

Example:

```text
755

Owner → rwx

Group → r-x

Others → r-x
```

---

## Production Example

A deployment script:

```text
deploy.sh
```

has:

```text
755
```

allowing everyone to execute it while only the owner can modify it.

---

## Production Tip

Grant only the permissions required. Avoid giving write permissions to users who do not need them.

---

## Interview Question

### Question

What do the permission symbols `r`, `w`, and `x` represent?

### Answer

`r` allows reading, `w` allows writing, and `x` allows executing a file or accessing a directory.

---

## 📌 Quick Revision

| Topic | Key Concept |
|--------|-------------|
| Linux Permissions | Control access to files and directories |
| Users | Root, Regular, and System users |
| Groups | Shared permission management |
| File Ownership | Owner and Group assigned to each file |
| Permission Types | Read (`r`), Write (`w`), Execute (`x`) |
| Permission Categories | Owner, Group, Others |
| Numeric Permissions | 755, 644, 700, etc. |

---

# 6. chmod (Change File Permissions)

## Answer

The `chmod` command is used to **change the permissions** of files and directories.

It controls who can:

- Read
- Write
- Execute

---

## Syntax

```bash
chmod [permissions] <file>
```

---

## Numeric Permission Examples

```bash
chmod 755 script.sh
```

```bash
chmod 644 config.txt
```

```bash
chmod 700 private.key
```

---

## Symbolic Permission Examples

Add execute permission:

```bash
chmod +x script.sh
```

Remove write permission:

```bash
chmod -w file.txt
```

Grant read permission to group:

```bash
chmod g+r report.txt
```

---

## Permission Mapping

| Number | Permission |
|---------|------------|
| 7 | rwx |
| 6 | rw- |
| 5 | r-x |
| 4 | r-- |
| 0 | --- |

Example:

```text
755

Owner  → rwx

Group  → r-x

Others → r-x
```

---

## Internal Workflow

```text
File

↓

chmod

↓

Updated Permissions

↓

Access Granted / Denied
```

---

## Production Example

A deployment script is given execute permission:

```bash
chmod 755 deploy.sh
```

allowing administrators to execute it while preventing unauthorized modifications by other users.

---

## Production Tip

Avoid using:

```bash
chmod 777
```

unless absolutely necessary, as it grants full access to everyone and introduces significant security risks.

---

## Interview Question

### Question

What is the purpose of the `chmod` command?

### Answer

The `chmod` command changes the read, write, and execute permissions of files and directories.

---

# 7. chown (Change Ownership)

## Answer

The `chown` command changes the **owner** of a file or directory.

It can also update both the owner and group simultaneously.

---

## Syntax

```bash
chown <owner> <file>
```

---

## Examples

Change owner:

```bash
sudo chown john file.txt
```

Change owner and group:

```bash
sudo chown john:developers file.txt
```

Apply recursively:

```bash
sudo chown -R nginx:nginx /var/www/html
```

---

## Internal Workflow

```text
File

↓

chown

↓

New Owner

↓

Access Control Updated
```

---

## Production Example

After deploying a web application, ownership of the application directory is assigned to the web server user:

```bash
sudo chown -R www-data:www-data /var/www/html
```

---

## Production Tip

Always verify ownership after deployments or file transfers to avoid application permission errors.

---

## Interview Question

### Question

What does the `chown` command do?

### Answer

The `chown` command changes the owner of a file or directory and can also change its associated group.

---

# 8. chgrp (Change Group)

## Answer

The `chgrp` command changes the **group ownership** of a file or directory.

This is useful when multiple users need shared access through a common group.

---

## Syntax

```bash
chgrp <group> <file>
```

---

## Example

```bash
sudo chgrp developers project.txt
```

Recursive example:

```bash
sudo chgrp -R developers /projects
```

---

## Internal Workflow

```text
File

↓

chgrp

↓

New Group

↓

Shared Access Updated
```

---

## Production Example

All deployment scripts are assigned to the `devops` group so every DevOps engineer can access them without changing individual user permissions.

---

## Production Tip

Use groups to simplify permission management instead of assigning permissions to each user separately.

---

## Interview Question

### Question

What is the purpose of the `chgrp` command?

### Answer

The `chgrp` command changes the group ownership of files and directories.

---

# 9. umask (Default Permissions)

## Answer

The **`umask`** command defines the **default permissions** assigned to newly created files and directories.

Instead of granting permissions, it **removes permissions** from the default values.

---

## Default Permissions

New File

```text
666

rw-rw-rw-
```

New Directory

```text
777

rwxrwxrwx
```

---

## Example

Current umask:

```bash
umask
```

Output:

```text
022
```

Resulting default permissions:

File:

```text
644

rw-r--r--
```

Directory:

```text
755

rwxr-xr-x
```

---

## Workflow

```text
Create File

↓

Default Permissions

↓

Apply umask

↓

Final Permissions
```

---

## Production Example

A server uses:

```text
umask 027
```

ensuring newly created files are not accessible to unauthorized users.

---

## Production Tip

Configure an appropriate `umask` for shared systems to prevent sensitive files from being created with overly permissive access.

---

## Interview Question

### Question

What is the purpose of `umask`?

### Answer

`umask` defines the default permissions for newly created files and directories by masking specific permission bits.

---

# 10. Practical Permission Examples

## Answer

The following permission values are commonly used in production environments.

---

## 755

```text
rwxr-xr-x
```

Used for:

- Shell Scripts
- Application Directories
- Executable Programs

---

## 644

```text
rw-r--r--
```

Used for:

- Configuration Files
- Source Code
- Text Files

---

## 700

```text
rwx------
```

Used for:

- Private Scripts
- SSH Directories
- Sensitive Files

---

## 600

```text
rw-------
```

Used for:

- SSH Private Keys
- Password Files
- Certificates

---

## Example

```bash
chmod 600 ~/.ssh/id_rsa
```

---

## Production Example

A private SSH key is assigned:

```text
600
```

to ensure only its owner can read or modify it.

---

## Production Tip

Use the **least permissive** settings that still allow the application or user to function correctly.

---

## Interview Question

### Question

Why should an SSH private key have `600` permissions?

### Answer

`600` ensures that only the file owner can read and modify the private key, protecting it from unauthorized access.

---

## 📌 Quick Revision

| Command | Purpose |
|----------|---------|
| `chmod` | Change file or directory permissions |
| `chown` | Change file owner |
| `chgrp` | Change group ownership |
| `umask` | Define default permissions |
| `755` | Executable files and directories |
| `644` | Standard files |
| `700` | Private directories/scripts |
| `600` | Sensitive files such as SSH private keys |

---

# 11. Access Control Lists (ACLs)

## Answer

**Access Control Lists (ACLs)** provide a more flexible permission model than traditional Linux permissions.

Standard Linux permissions allow access control only for:

- Owner
- Group
- Others

ACLs allow permissions to be assigned to **specific users and groups** without changing the file owner or primary group.

---

## ACL Architecture

```text
File

↓

Owner

↓

Group

↓

ACL Entries

↓

Specific Users & Groups
```

---

## View ACL

```bash
getfacl file.txt
```

---

## Set ACL

Grant read and write permission to user `john`:

```bash
setfacl -m u:john:rw file.txt
```

Grant read permission to group `developers`:

```bash
setfacl -m g:developers:r file.txt
```

Remove an ACL entry:

```bash
setfacl -x u:john file.txt
```

---

## Production Example

A shared project file is owned by the `devops` group, but a security auditor needs read-only access.

Instead of changing ownership, an ACL grants the auditor read permission.

---

## Production Tip

Use ACLs when a few users require exceptions to standard ownership and permission rules.

---

## Interview Question

### Question

What is an ACL in Linux?

### Answer

An ACL (Access Control List) extends standard Linux permissions by allowing specific users and groups to have customized access to files and directories.

---

# 12. SUID (Set User ID)

## Answer

**SUID (Set User ID)** allows a user to execute a program with the permissions of the file owner instead of the current user.

This is commonly used for programs that require temporary elevated privileges.

---

## Example Permission

```text
-rwsr-xr-x
```

Notice the **`s`** replacing the owner's execute permission.

---

## Find SUID Files

```bash
find / -perm -4000
```

---

## Architecture

```text
User

↓

Execute Program

↓

Program Runs as File Owner

↓

Task Completed
```

---

## Production Example

The `passwd` command uses SUID so users can change their own passwords even though the password database is owned by `root`.

---

## Production Tip

Review SUID programs regularly because unnecessary SUID binaries can increase security risks.

---

## Interview Question

### Question

What is SUID?

### Answer

SUID allows a program to run with the permissions of the file owner instead of the user executing it.

---

# 13. SGID (Set Group ID)

## Answer

**SGID (Set Group ID)** behaves differently for files and directories.

### On Files

Programs execute with the file's group permissions.

### On Directories

New files inherit the directory's group ownership automatically.

---

## Example Permission

```text
-rwxr-sr-x
```

or for a directory:

```text
drwxrwsr-x
```

---

## Find SGID Files

```bash
find / -perm -2000
```

---

## Architecture

```text
Directory

↓

New File Created

↓

Inherited Group

↓

Shared Collaboration
```

---

## Production Example

A shared application directory has the SGID bit set so every file created automatically belongs to the `developers` group.

---

## Production Tip

Use SGID on shared project directories to simplify team collaboration.

---

## Interview Question

### Question

What is SGID?

### Answer

SGID allows files to run with the file's group permissions and causes new files in a directory to inherit the directory's group ownership.

---

# 14. Sticky Bit

## Answer

The **Sticky Bit** prevents users from deleting files owned by other users inside a shared directory.

Only the following can delete files:

- File Owner
- Directory Owner
- Root User

---

## Example Permission

```text
drwxrwxrwt
```

Notice the **`t`** in the execute position.

---

## Find Sticky Bit Directories

```bash
find / -perm -1000
```

---

## Common Example

```text
/tmp
```

The `/tmp` directory is writable by everyone, but the Sticky Bit ensures users cannot delete each other's files.

---

## Architecture

```text
Shared Directory

↓

Multiple Users

↓

Sticky Bit

↓

Only Owner Can Delete Own Files
```

---

## Production Example

A shared upload directory uses the Sticky Bit to prevent users from accidentally deleting each other's uploaded files.

---

## Production Tip

Enable the Sticky Bit on shared writable directories to improve security.

---

## Interview Question

### Question

What is the purpose of the Sticky Bit?

### Answer

The Sticky Bit prevents users from deleting files owned by other users in shared directories.

---

# 15. Common Permission Mistakes

## Mistake 1

❌ Using:

```bash
chmod 777
```

This grants full access to everyone and should be avoided.

---

## Mistake 2

❌ Running applications as the `root` user.

Instead, run applications using dedicated service accounts.

---

## Mistake 3

❌ Assigning incorrect ownership after deployments.

Always verify:

```bash
ls -l
```

---

## Mistake 4

❌ Ignoring ACLs when fine-grained permissions are required.

ACLs are often a better solution than changing ownership.

---

## Mistake 5

❌ Leaving unnecessary SUID binaries on production systems.

Audit them regularly.

---

## Production Tip

Review file permissions periodically to identify excessive privileges and remove unnecessary access.

---

## Interview Question

### Question

What are common Linux permission mistakes?

### Answer

Common mistakes include using `chmod 777`, running applications as root, assigning incorrect ownership, ignoring ACLs, and leaving unnecessary SUID binaries.

---

# 16. Linux Permission Best Practices

## Answer

Proper permission management is essential for securing Linux systems.

---

## User Management

✅ Use non-root accounts.

---

## Permissions

✅ Grant the minimum permissions required.

---

## Ownership

✅ Verify file ownership after deployments.

---

## Shared Access

✅ Use groups or ACLs instead of granting permissions to individual users whenever possible.

---

## Security

✅ Audit SUID and SGID files regularly.

---

## Monitoring

✅ Periodically review:

- File Permissions
- Ownership
- ACLs

---

## Production Example

A DevOps team:

- Uses dedicated service accounts
- Stores SSH keys with `600` permissions
- Uses SGID for shared directories
- Enables the Sticky Bit on `/tmp`
- Audits permissions during security reviews

---

## Production Tip

Implement the **Principle of Least Privilege (PoLP)** across users, groups, applications, and services.

---

## Interview Question

### Question

What are Linux permission best practices?

### Answer

Use least privilege, avoid `chmod 777`, verify ownership, manage shared access through groups or ACLs, and regularly audit permissions.

---

# 📌 Quick Revision

### Permission Model

```text
Owner

↓

Group

↓

Others
```

---

### Special Permissions

```text
SUID

↓

Run as File Owner

----------------------

SGID

↓

Inherit Group

----------------------

Sticky Bit

↓

Protect Shared Files
```

---

### Common Commands

```bash
chmod

chown

chgrp

umask

getfacl

setfacl

find / -perm -4000

find / -perm -2000

find / -perm -1000
```

---

# Summary

After completing this guide, you should understand:

✅ Linux Users

✅ Linux Groups

✅ File Ownership

✅ File Permissions

✅ chmod

✅ chown

✅ chgrp

✅ umask

✅ Access Control Lists (ACLs)

✅ SUID

✅ SGID

✅ Sticky Bit

✅ Permission Best Practices

---

# Interview Quick Revision

| Question | One-Line Answer |
|----------|-----------------|
| What are Linux permissions? | They control read, write, and execute access to files and directories |
| What does `chmod` do? | Changes file or directory permissions |
| What does `chown` do? | Changes file ownership |
| What does `chgrp` do? | Changes group ownership |
| What is `umask`? | Defines default permissions for new files and directories |
| What is an ACL? | An extended permission system for specific users and groups |
| What is SUID? | Runs a program with the file owner's privileges |
| What is SGID? | Causes files to inherit the directory's group or runs a program with the file's group |
| What is the Sticky Bit? | Prevents users from deleting other users' files in shared directories |
| Why should `chmod 777` be avoided? | It grants unrestricted access to everyone, creating serious security risks |

---

# 🎉 Linux Permissions Module Completed

Congratulations! You have completed **Linux Permissions**, including:

- ✅ Linux Users
- ✅ Linux Groups
- ✅ File Ownership
- ✅ Linux Permissions
- ✅ chmod
- ✅ chown
- ✅ chgrp
- ✅ umask
- ✅ Access Control Lists (ACLs)
- ✅ SUID
- ✅ SGID
- ✅ Sticky Bit
- ✅ Linux Permission Best Practices

You now have a strong understanding of Linux access control, security permissions, and user management—skills essential for Linux Administration, DevOps, Cloud Engineering, and Site Reliability Engineering (SRE).

---

# 🚀 What's Next?

➡️ **04-linux-process-management.md**

In the next guide, you'll learn:

- What is a Process?
- Process Lifecycle
- Foreground vs Background Processes
- Process States
- `ps`, `top`, `htop`
- `kill`, `killall`, `pkill`
- `nice` & `renice`
- `systemd` & Service Management
- Production Best Practices
- Common Interview Questions