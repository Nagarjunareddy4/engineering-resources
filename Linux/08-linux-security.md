# Linux Security

This guide explains everything you need to know about Linux Security.

If you've ever wondered:

- What is Linux Security?
- Why is Linux considered secure?
- What is Authentication and Authorization?
- How are Users and Groups secured?
- How can SSH be secured?
- Why is Linux Security important?

This guide is for you.

---

# 📑 Table of Contents

1. What is Linux Security?
2. Authentication
3. Authorization
4. User & Group Security
5. SSH Security
6. Common Interview Questions

---

# 1. What is Linux Security?

## Answer

**Linux Security** is the collection of mechanisms, tools, and best practices used to protect Linux systems from unauthorized access, misuse, malware, and cyberattacks.

Linux security is based on multiple layers including:

- User Authentication
- Access Control
- File Permissions
- Firewalls
- Encryption
- Security Policies
- System Monitoring

---

## Security Architecture

```text
User

↓

Authentication

↓

Authorization

↓

Linux Kernel

↓

Applications

↓

System Resources
```

---

## Key Characteristics

Linux Security provides:

- Confidentiality
- Integrity
- Availability
- Least Privilege
- Secure Access Control

---

## Real World Example

A production web server:

- Allows SSH access only through key authentication
- Uses a firewall to allow only ports 22, 80, and 443
- Runs services using non-root users
- Stores sensitive data with restricted permissions

---

## Benefits

Linux Security helps:

- Prevent Unauthorized Access
- Protect Sensitive Data
- Reduce Security Risks
- Ensure Compliance
- Improve System Reliability

---

## Production Tip

Security should be implemented in layers. Never rely on a single security mechanism.

---

## Interview Question

### Question

What is Linux Security?

### Answer

Linux Security is the set of mechanisms that protect Linux systems by controlling access, securing data, monitoring activity, and preventing unauthorized actions.

---

# 2. Authentication

## Answer

**Authentication** verifies the identity of a user or service before allowing access to the system.

Simply put:

> **Authentication answers the question: "Who are you?"**

---

## Common Authentication Methods

- Username & Password
- SSH Keys
- Multi-Factor Authentication (MFA)
- LDAP / Active Directory
- Kerberos

---

## Authentication Workflow

```text
User

↓

Username

↓

Password / SSH Key

↓

Authentication

↓

Access Granted
```

---

## Important Files

User accounts

```text
/etc/passwd
```

Encrypted passwords

```text
/etc/shadow
```

Groups

```text
/etc/group
```

---

## Production Example

A DevOps engineer connects to a production server using an SSH private key instead of a password.

---

## Production Tip

Always prefer SSH key authentication over password-based authentication for production systems.

---

## Interview Question

### Question

What is Authentication?

### Answer

Authentication is the process of verifying the identity of a user or service before granting access.

---

# 3. Authorization

## Answer

**Authorization** determines **what an authenticated user is allowed to do**.

Simply put:

> **Authorization answers the question: "What are you allowed to access?"**

---

## Authorization Workflow

```text
Authentication Successful

↓

Permission Check

↓

Allow or Deny Access
```

---

## Authorization Depends On

- User
- Group
- File Permissions
- ACLs
- Security Policies

---

## Example

A developer successfully logs into a server.

Authentication succeeds.

However, the developer cannot modify:

```text
/etc/passwd
```

because authorization denies write access.

---

## Production Example

Application users can read log files but only administrators can modify system configuration files.

---

## Production Tip

Follow the **Principle of Least Privilege (PoLP)** by granting only the permissions required to perform a job.

---

## Interview Question

### Question

What is the difference between Authentication and Authorization?

### Answer

Authentication verifies identity, while Authorization determines what actions an authenticated user is permitted to perform.

---

# 4. User & Group Security

## Answer

Linux secures systems by assigning permissions through users and groups.

Each process runs under a user account and inherits that user's permissions.

---

## User Types

### Root User

- UID 0
- Full administrative access

---

### Regular User

Limited permissions.

Used for daily work.

---

### System User

Used by applications and services.

Examples:

```text
nginx

mysql

postgres

www-data
```

---

## Security Model

```text
Users

↓

Groups

↓

Permissions

↓

Resources
```

---

## Useful Commands

Current user

```bash
whoami
```

User information

```bash
id
```

Groups

```bash
groups
```

---

## Production Example

Instead of running NGINX as root, it runs using the dedicated:

```text
www-data
```

user to minimize security risks.

---

## Production Tip

Never run applications as the root user unless absolutely necessary.

---

## Interview Question

### Question

Why are system users used in Linux?

### Answer

System users isolate applications from administrative accounts and reduce the impact of security vulnerabilities.

---

# 5. SSH Security

## Answer

SSH (Secure Shell) provides encrypted remote access to Linux systems.

Proper SSH configuration is one of the most important aspects of Linux security.

---

## Secure SSH Practices

✅ Use SSH Keys

✅ Disable Root Login

✅ Disable Password Authentication (where possible)

✅ Use Strong Passphrases

✅ Change Default Port (optional)

✅ Enable MFA (if supported)

---

## SSH Workflow

```text
Administrator

↓

SSH Client

↓

Encrypted Connection

↓

Linux Server
```

---

## Important SSH Configuration

Configuration file

```text
/etc/ssh/sshd_config
```

---

## Useful Commands

Generate SSH key

```bash
ssh-keygen
```

Connect using SSH

```bash
ssh user@server
```

Copy public key

```bash
ssh-copy-id user@server
```

Restart SSH service

```bash
sudo systemctl restart sshd
```

---

## Production Example

Production servers:

- Allow only SSH key authentication
- Disable root login
- Restrict SSH access using firewall rules
- Log all SSH login attempts

---

## Production Tip

Disable direct root SSH login by setting:

```text
PermitRootLogin no
```

in the SSH server configuration.

---

## Interview Question

### Question

How can SSH security be improved?

### Answer

Use SSH keys, disable root login, restrict password authentication, enable MFA when available, and limit access using firewalls.

---

## 📌 Quick Revision

| Topic | Key Concept |
|--------|-------------|
| Linux Security | Protects systems from unauthorized access |
| Authentication | Verifies identity |
| Authorization | Controls permissions |
| Root User | Full administrative access |
| System Users | Run applications securely |
| SSH | Secure remote access |
| `/etc/passwd` | User account information |
| `/etc/shadow` | Encrypted password storage |
| `/etc/group` | Group information |
| `sshd_config` | SSH server configuration |

---

# 6. Linux Firewalls

## Answer

A **Firewall** controls incoming and outgoing network traffic based on predefined security rules.

It protects Linux systems by allowing only authorized network connections.

---

## Firewall Workflow

```text
Incoming Traffic

↓

Firewall Rules

↓

Allow or Block

↓

Linux System
```

---

## Common Linux Firewalls

| Firewall | Description |
|----------|-------------|
| iptables | Traditional Linux firewall |
| firewalld | Dynamic firewall management (RHEL/CentOS) |
| ufw | Uncomplicated Firewall (Ubuntu) |
| nftables | Modern replacement for iptables |

---

## UFW Commands (Ubuntu)

Enable firewall

```bash
sudo ufw enable
```

Check status

```bash
sudo ufw status
```

Allow SSH

```bash
sudo ufw allow 22
```

Allow HTTP

```bash
sudo ufw allow 80
```

Allow HTTPS

```bash
sudo ufw allow 443
```

Deny a port

```bash
sudo ufw deny 23
```

---

## firewalld Commands (RHEL)

View status

```bash
sudo firewall-cmd --state
```

Allow HTTP

```bash
sudo firewall-cmd --permanent --add-service=http
```

Reload rules

```bash
sudo firewall-cmd --reload
```

---

## Production Example

A production web server allows only:

- SSH (22)
- HTTP (80)
- HTTPS (443)

All other inbound ports remain blocked.

---

## Production Tip

Follow a **default deny** strategy—block all inbound traffic except the ports explicitly required.

---

## Interview Question

### Question

Why is a firewall important in Linux?

### Answer

A firewall protects Linux systems by filtering network traffic and allowing only authorized connections.

---

# 7. SELinux (Security-Enhanced Linux)

## Answer

**SELinux** provides **Mandatory Access Control (MAC)**, adding an extra layer of security beyond standard Linux file permissions.

Even if file permissions allow access, SELinux can still deny it based on security policies.

---

## SELinux Modes

| Mode | Description |
|------|-------------|
| Enforcing | Policies are enforced |
| Permissive | Violations are logged but not blocked |
| Disabled | SELinux is disabled |

---

## Check SELinux Status

```bash
getenforce
```

Detailed status

```bash
sestatus
```

---

## Change Mode (Temporary)

Permissive

```bash
sudo setenforce 0
```

Enforcing

```bash
sudo setenforce 1
```

---

## Architecture

```text
Application

↓

SELinux Policy

↓

Allow or Deny

↓

Resource Access
```

---

## Production Example

An administrator deploys a web application. Although file permissions are correct, SELinux blocks access until the proper security context is applied.

---

## Production Tip

Avoid disabling SELinux in production. Investigate and correct policy or context issues instead.

---

## Interview Question

### Question

What is SELinux?

### Answer

SELinux is a Mandatory Access Control (MAC) system that enforces security policies to restrict application and user access beyond traditional Linux permissions.

---

# 8. AppArmor

## Answer

**AppArmor** is another Linux security framework that restricts application capabilities using security profiles.

Unlike SELinux, AppArmor primarily uses **path-based** policies.

It is commonly used on Ubuntu and SUSE systems.

---

## Check AppArmor Status

```bash
sudo aa-status
```

---

## Common Modes

| Mode | Description |
|------|-------------|
| Enforce | Policies are enforced |
| Complain | Violations are logged only |
| Disable | Profile disabled |

---

## Workflow

```text
Application

↓

AppArmor Profile

↓

Allow or Deny

↓

System Resource
```

---

## Production Example

An application is confined by an AppArmor profile so that even if it is compromised, it cannot access sensitive system files.

---

## Production Tip

Review AppArmor profiles after application upgrades to ensure they continue to match application behavior.

---

## Interview Question

### Question

What is the difference between SELinux and AppArmor?

### Answer

SELinux uses security labels and Mandatory Access Control, while AppArmor secures applications using path-based profiles.

---

# 9. File & Directory Security

## Answer

Protecting files and directories is fundamental to Linux security.

Security is achieved through:

- Ownership
- Permissions
- ACLs
- Encryption
- Monitoring

---

## Useful Commands

View permissions

```bash
ls -l
```

Change permissions

```bash
chmod
```

Change ownership

```bash
chown
```

View ACLs

```bash
getfacl file.txt
```

---

## Sensitive Files

```text
/etc/shadow

/etc/passwd

/etc/sudoers
```

---

## Production Example

Private SSH keys are stored with:

```text
600
```

permissions to ensure only the owner can read them.

---

## Production Tip

Regularly audit sensitive files for incorrect ownership or excessive permissions.

---

## Interview Question

### Question

Why are file permissions important?

### Answer

File permissions protect sensitive data by controlling who can read, modify, or execute files and directories.

---

# 10. Password Policies

## Answer

Strong password policies reduce the risk of unauthorized access.

Organizations often enforce:

- Minimum password length
- Complexity requirements
- Password expiration
- Password history
- Account lockout

---

## Useful Commands

View password aging

```bash
chage -l username
```

Set password expiry

```bash
sudo chage -M 90 username
```

Force password change at next login

```bash
sudo passwd -e username
```

---

## Workflow

```text
User

↓

Password Policy

↓

Authentication

↓

Access Granted
```

---

## Production Example

Employees must change passwords every 90 days and cannot reuse their last five passwords.

---

## Production Tip

Where possible, combine strong password policies with Multi-Factor Authentication (MFA).

---

## Interview Question

### Question

Why are password policies important?

### Answer

Password policies reduce the risk of unauthorized access by enforcing strong and regularly updated credentials.

---

# 11. Account Locking

## Answer

Account locking protects systems against brute-force login attempts.

After a configured number of failed login attempts, the account is temporarily locked.

---

## View Failed Login Attempts

```bash
lastb
```

---

## Unlock User (Example)

```bash
sudo passwd -u username
```

---

## Workflow

```text
Failed Login Attempts

↓

Threshold Reached

↓

Account Locked

↓

Administrator Unlocks
```

---

## Production Example

An attacker attempts multiple password guesses. The account is automatically locked after five failed login attempts, preventing further access.

---

## Production Tip

Use tools such as **fail2ban** or **pam_tally2/faillock** (depending on your distribution) to automatically detect and block repeated authentication failures.

---

## Interview Question

### Question

Why is account locking used?

### Answer

Account locking helps protect Linux systems from brute-force password attacks by temporarily disabling accounts after repeated failed login attempts.

---

# 📌 Quick Revision

| Topic | Key Concept |
|--------|-------------|
| Firewall | Controls network traffic |
| `ufw` | Firewall for Ubuntu |
| `firewalld` | Firewall management on RHEL-based systems |
| SELinux | Mandatory Access Control |
| AppArmor | Path-based application security |
| File Security | Ownership and permissions |
| Password Policies | Enforce strong authentication |
| Account Locking | Prevent brute-force attacks |

---

# 12. System Hardening

## Answer

**System Hardening** is the process of reducing a system's attack surface by removing unnecessary services, securing configurations, and applying security best practices.

The goal is to make a Linux system as secure as possible before deploying it to production.

---

## Hardening Workflow

```text
Install Linux

↓

Apply Updates

↓

Remove Unused Packages

↓

Secure SSH

↓

Configure Firewall

↓

Monitor System
```

---

## Common Hardening Tasks

- Apply security updates
- Disable unused services
- Disable root SSH login
- Use SSH key authentication
- Configure a firewall
- Remove unnecessary packages
- Enforce strong password policies
- Enable security logging

---

## Useful Commands

Update packages

```bash
sudo apt update && sudo apt upgrade
```

or

```bash
sudo yum update
```

List running services

```bash
systemctl list-units --type=service
```

Disable a service

```bash
sudo systemctl disable service-name
```

---

## Production Example

A newly provisioned Linux server is hardened by:

- Installing updates
- Enabling a firewall
- Disabling Telnet
- Disabling root SSH login
- Configuring automatic security updates

before hosting any production applications.

---

## Production Tip

Perform system hardening immediately after installing the operating system and before exposing the server to public networks.

---

## Interview Question

### Question

What is Linux system hardening?

### Answer

System hardening is the process of securing a Linux system by reducing its attack surface through secure configurations, updates, and removal of unnecessary services.

---

# 13. Security Monitoring

## Answer

Security monitoring involves continuously observing system activity to detect suspicious behavior, unauthorized access, or security incidents.

---

## Monitoring Workflow

```text
System Activity

↓

Logs

↓

Monitoring Tools

↓

Alerts

↓

Administrator
```

---

## Common Log Files

Authentication logs

```text
/var/log/auth.log
```

(RHEL-based systems may use `/var/log/secure`.)

System logs

```text
/var/log/syslog
```

Kernel logs

```text
dmesg
```

---

## Useful Commands

View authentication logs

```bash
sudo tail -f /var/log/auth.log
```

View system journal

```bash
journalctl
```

View failed logins

```bash
lastb
```

View login history

```bash
last
```

---

## Production Example

A security team monitors authentication logs and receives alerts when repeated failed SSH login attempts occur.

---

## Production Tip

Centralize logs using tools such as the ELK Stack, Loki, or Splunk for easier analysis and long-term retention.

---

## Interview Question

### Question

Why is security monitoring important?

### Answer

Security monitoring helps detect suspicious activities, security breaches, and system anomalies before they become major incidents.

---

# 14. Common Security Mistakes

## Mistake 1

❌ Logging in as `root` for daily administration.

Use a regular user with `sudo` privileges instead.

---

## Mistake 2

❌ Using weak passwords.

Enforce strong password policies and use Multi-Factor Authentication where possible.

---

## Mistake 3

❌ Leaving unnecessary ports open.

Allow only required services through the firewall.

---

## Mistake 4

❌ Disabling SELinux or AppArmor without understanding the consequences.

Investigate policy issues instead of disabling security controls.

---

## Mistake 5

❌ Ignoring security updates.

Regularly apply security patches to reduce vulnerabilities.

---

## Production Tip

Most successful attacks exploit known vulnerabilities that already have available security patches.

---

## Interview Question

### Question

What are common Linux security mistakes?

### Answer

Common mistakes include using the root account for daily work, weak passwords, leaving unnecessary ports open, disabling security mechanisms, and failing to install updates.

---

# 15. Linux Security Best Practices

## Answer

Following security best practices helps protect Linux systems from unauthorized access and cyberattacks.

---

## User Management

✅ Use non-root accounts.

---

## Authentication

✅ Use SSH key authentication.

---

## Authorization

✅ Apply the Principle of Least Privilege (PoLP).

---

## Firewall

✅ Allow only required network ports.

---

## Updates

✅ Install security patches regularly.

---

## Monitoring

✅ Review authentication logs and monitor system activity.

---

## Backups

✅ Perform regular backups and test recovery procedures.

---

## Documentation

✅ Document security policies, firewall rules, user access, and incident response procedures.

---

## Production Example

A production environment:

- Uses SSH keys
- Disables root login
- Enables SELinux
- Restricts inbound traffic with a firewall
- Monitors logs using a centralized logging platform
- Applies automatic security updates

---

## Production Tip

Security is an ongoing process. Regular audits, monitoring, and updates are just as important as the initial configuration.

---

## Interview Question

### Question

What are Linux security best practices?

### Answer

Use least privilege, secure SSH, enable firewalls, apply updates, monitor logs, perform backups, and continuously review system security.

---

# 16. Incident Response Basics

## Answer

**Incident Response** is the structured process of identifying, containing, investigating, and recovering from security incidents.

---

## Incident Response Lifecycle

```text
Preparation

↓

Detection

↓

Containment

↓

Eradication

↓

Recovery

↓

Lessons Learned
```

---

## Incident Response Steps

1. Identify the incident.
2. Contain the threat.
3. Investigate the root cause.
4. Remove malicious activity.
5. Restore normal operations.
6. Document findings and improve defenses.

---

## Production Example

An administrator detects repeated unauthorized SSH login attempts:

- Blocks the attacker's IP address.
- Reviews authentication logs.
- Rotates compromised credentials if necessary.
- Restores secure access.
- Updates firewall and monitoring rules.

---

## Production Tip

Maintain documented incident response procedures so the team can react quickly during security events.

---

## Interview Question

### Question

What are the main phases of incident response?

### Answer

Preparation, Detection, Containment, Eradication, Recovery, and Lessons Learned.

---

# 📌 Quick Revision

### Linux Security Layers

```text
Authentication

↓

Authorization

↓

Permissions

↓

Firewall

↓

SELinux / AppArmor

↓

Monitoring

↓

Incident Response
```

---

### Important Commands

```bash
whoami

id

ssh-keygen

ufw status

firewall-cmd --state

getenforce

sestatus

aa-status

journalctl

last

lastb

systemctl
```

---

# Summary

After completing this guide, you should understand:

✅ Linux Security Fundamentals

✅ Authentication

✅ Authorization

✅ User & Group Security

✅ SSH Security

✅ Linux Firewalls

✅ SELinux

✅ AppArmor

✅ File & Directory Security

✅ Password Policies

✅ Account Locking

✅ System Hardening

✅ Security Monitoring

✅ Linux Security Best Practices

✅ Incident Response Basics

---

# Interview Quick Revision

| Question | One-Line Answer |
|----------|-----------------|
| What is Linux Security? | The set of mechanisms used to protect Linux systems from unauthorized access and attacks |
| What is Authentication? | Verifies the identity of a user or service |
| What is Authorization? | Determines what an authenticated user is allowed to do |
| What is SELinux? | A Mandatory Access Control system that enforces security policies |
| What is AppArmor? | A path-based application security framework |
| Why is SSH preferred? | It provides encrypted remote access |
| What is system hardening? | Reducing the attack surface by securing configurations and removing unnecessary services |
| Why is security monitoring important? | It helps detect suspicious activity and potential security incidents |
| What are Linux security best practices? | Use least privilege, secure SSH, firewalls, updates, monitoring, and backups |
| What are the phases of incident response? | Preparation, Detection, Containment, Eradication, Recovery, and Lessons Learned |

---

# 🎉 Linux Security Module Completed

Congratulations! You have completed **Linux Security**, including:

- ✅ Linux Security Fundamentals
- ✅ Authentication & Authorization
- ✅ User & Group Security
- ✅ SSH Security
- ✅ Linux Firewalls (`iptables`, `firewalld`, `ufw`)
- ✅ SELinux & AppArmor
- ✅ File & Directory Security
- ✅ Password Policies
- ✅ Account Locking
- ✅ System Hardening
- ✅ Security Monitoring
- ✅ Linux Security Best Practices
- ✅ Incident Response Basics

You now have a strong understanding of Linux security principles and operational security practices—essential skills for Linux Administration, DevOps, Cloud Engineering, DevSecOps, and Site Reliability Engineering (SRE).

---

# 🚀 What's Next?

➡️ **09-linux-interview-questions.md**

In the final Linux module, you'll learn:

- Top Linux Interview Questions
- Beginner, Intermediate & Advanced Questions
- Scenario-Based Questions
- Troubleshooting Questions
- DevOps & Cloud-Focused Linux Questions
- Production Scenarios
- Expert Tips for Interviews