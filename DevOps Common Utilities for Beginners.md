# 🚀 DevOps Common Utilities for Beginners

> **A practical guide to the most commonly used Linux utilities every DevOps Engineer should know.**

---

## 📚 Table of Contents

- [Introduction](#-introduction)
- [Why do we install these utilities?](#-why-do-we-install-these-utilities)
- [1. apt](#1-apt)
- [2. What does -y mean?](#2-what-does--y-mean)
- [3. curl](#3-curl)
- [4. wget](#4-wget)
- [5. Git](#5-git)
- [6. zip](#6-zip)
- [7. unzip](#7-unzip)
- [8. vim](#8-vim)
- [9. jq](#9-jq)
- [10. tree](#10-tree)
- [11. net-tools](#11-net-tools)
- [12. ca-certificates](#12-ca-certificates)
- [13. software-properties-common](#13-software-properties-common)
- [14. apt-transport-https](#14-apt-transport-https)
- [15. gnupg](#15-gnupg)
- [16. lsb-release](#16-lsb-release)
- [Complete Installation Command](#-complete-installation-command)
- [Real World Example](#-real-world-example)
- [Summary](#-summary)

---

# 📖 Introduction

Whenever a DevOps Engineer receives a new Linux Virtual Machine (VM), it usually contains only the operating system.

Before we can start working with:

- Java Applications
- Docker
- Kubernetes
- Jenkins
- Azure DevOps
- Terraform
- Ansible
- Cloud Platforms

We first need to install some essential utilities.

Think of these tools as your **DevOps Toolbox**.

```
           Fresh Ubuntu VM
                  │
                  ▼
      Install Common Utilities
                  │
                  ▼
       Ready for DevOps Projects
```

---

# 🎯 Why do we install these utilities?

Imagine you purchased a brand-new laptop.

The laptop comes with Windows installed.

But can you start developing immediately?

No.

You need to install:

- Chrome
- VS Code
- Git
- Java
- Docker

Linux is exactly the same.

A newly created VM only contains the operating system.

We install these utilities because every DevOps project depends on them.

---

# 1. apt

## What is apt?

APT stands for

> **Advanced Package Tool**

It is Ubuntu's package manager.

It helps us install, remove, update, and upgrade software.

---

## Why do we use it?

Instead of manually downloading software from websites,

APT downloads software directly from Ubuntu repositories.

---

### Update package list

```bash
sudo apt update
```

What happens?

```
Ubuntu Repository
        │
        ▼
Download Latest Package Information
        │
        ▼
Your VM knows the latest software versions
```

Think of this as refreshing your Play Store before installing apps.

---

### Upgrade installed packages

```bash
sudo apt upgrade
```

Example

Before

```
Git 2.34
Docker 24
```

After Upgrade

```
Git 2.50
Docker 28
```

---

### Install software

```bash
sudo apt install git
```

This downloads Git from Ubuntu Repository and installs it.

---

# 2. What does -y mean?

Normally Linux asks

```
Do you want to continue? [Y/n]
```

If you are installing software manually,

you can type

```
Y
```

But imagine running a script on 100 servers.

Nobody will sit and type Y 100 times.

That's why we use

```bash
sudo apt install -y git
```

The **-y** means

> Automatically answer YES for every installation prompt.

---

Without -y

```
Install Git?
      │
      ▼
Waiting...
```

With -y

```
Install Git
      │
      ▼
Automatically Continue
```

This is why almost every automation script contains

```bash
apt install -y
```

---

# 3. curl

## What is curl?

curl is a command-line tool used to communicate with web servers.

It can

- Download files
- Call REST APIs
- Upload files
- Test APIs
- Download installation scripts

---

### Example

```bash
curl https://google.com
```

It downloads the HTML of Google's homepage.

---

### Real DevOps Example

Docker Installation

```bash
curl -fsSL https://get.docker.com | sh
```

Visual

```
Docker Website
       │
       ▼
curl downloads installation script
       │
       ▼
Shell executes it
       │
       ▼
Docker Installed
```

---

### Another Example

Calling an API

```bash
curl https://api.github.com/users/octocat
```

Returns JSON response.

---

# 4. wget

wget stands for

> Web Get

It is mainly used to download files.

---

Example

```bash
wget https://example.com/sample.zip
```

Visual

```
Internet
     │
     ▼
Download File
     │
     ▼
Current Directory
```

---

Download and rename

```bash
wget -O terraform.zip https://example.com/file.zip
```

---

# curl vs wget

| curl | wget |
|------|------|
| API Calls | File Downloads |
| Displays output | Saves files |
| Used in REST APIs | Used for downloading packages |
| Supports many HTTP operations | Best for downloading |

---

# 5. Git

Git is a Version Control System.

It tracks source code changes.

Without Git

```
Developer sends ZIP files ❌
```

With Git

```
GitHub
    │
    ▼
Clone
    │
    ▼
Local Machine
```

---

Clone repository

```bash
git clone https://github.com/user/project.git
```

---

Check changes

```bash
git status
```

---

Stage files

```bash
git add .
```

---

Commit changes

```bash
git commit -m "Added Login Feature"
```

---

Push code

```bash
git push
```

---

# 6. zip

Compresses files into a ZIP archive.

Without ZIP

```
Project Folder
500 MB
```

With ZIP

```
project.zip
150 MB
```

---

Example

```bash
zip project.zip project/
```

---

Zip multiple files

```bash
zip logs.zip app.log server.log
```

---

# 7. unzip

Extract ZIP files.

Example

```bash
unzip project.zip
```

Visual

```
project.zip
      │
      ▼
Extract
      │
      ▼
Project Folder
```

---

Extract to another folder

```bash
unzip project.zip -d /opt/project
```

---

# 8. vim

Vim is a terminal-based text editor.

Useful when servers don't have GUI.

Example

```bash
vim application.properties
```

Common usage

- Edit YAML
- Edit Dockerfile
- Edit Jenkinsfile
- Edit Shell Scripts
- Edit Kubernetes YAML

---

# 9. jq

jq is used to read and format JSON.

Without jq

```
{"name":"John","role":"DevOps","company":"ABC"}
```

Difficult to read.

---

With jq

```bash
cat user.json | jq
```

Output

```json
{
  "name": "John",
  "role": "DevOps",
  "company": "ABC"
}
```

---

Real DevOps Example

```bash
kubectl get pods -o json | jq
```

---

Azure CLI

```bash
az vm list | jq
```

---

AWS CLI

```bash
aws ec2 describe-instances | jq
```

---

# 10. tree

Displays folder structure visually.

Example

```bash
tree
```

Output

```
project/

├── Dockerfile
├── Jenkinsfile
├── README.md
├── terraform
│   ├── main.tf
│   ├── variables.tf
│   └── outputs.tf
│
├── kubernetes
│   ├── deployment.yaml
│   └── service.yaml
│
└── scripts
    └── bootstrap.sh
```

Much easier than running multiple ls commands.

---

# 11. net-tools

Provides networking utilities.

Common commands

```bash
ifconfig
```

or

```bash
netstat
```

Although modern Linux recommends

```bash
ip addr
```

Useful for

- IP Address
- Network Interfaces
- Port Information

---

# 12. ca-certificates

Contains trusted SSL Certificates.

Without it

```
Git Clone
     │
     ▼
SSL Error
```

With it

```
Git Clone
     │
     ▼
SSL Verified
     │
     ▼
Success
```

Required for

- Git
- Docker
- Azure CLI
- Terraform
- Kubernetes

---

# 13. software-properties-common

Installs

```
add-apt-repository
```

Example

```bash
sudo add-apt-repository ppa:git-core/ppa
```

Allows adding new repositories.

---

# 14. apt-transport-https

Allows apt to download packages over HTTPS.

Without it

```
HTTP Only
```

With it

```
HTTPS Secure Download
```

Most repositories today require HTTPS.

---

# 15. gnupg

Also called GPG.

Used for verifying software authenticity.

Example

Installing Terraform

```
HashiCorp
      │
Signs Package
      │
      ▼
Ubuntu verifies signature
      │
      ▼
Safe Installation
```

Protects against installing fake software.

---

# 16. lsb-release

Displays Linux Version Information.

Example

```bash
lsb_release -a
```

Output

```
Distributor ID: Ubuntu
Description: Ubuntu 24.04 LTS
Release: 24.04
Codename: noble
```

Very useful in automation scripts.

Example

```bash
if Ubuntu 22
Install Version A

if Ubuntu 24
Install Version B
```

---

# 🚀 Complete Installation Command

```bash
sudo apt update

sudo apt install -y \
curl \
wget \
git \
zip \
unzip \
vim \
jq \
tree \
net-tools \
ca-certificates \
software-properties-common \
apt-transport-https \
gnupg \
lsb-release
```

---

# 🎯 What happens after running this?

```
              Fresh Ubuntu VM
                     │
                     ▼
               apt update
                     │
                     ▼
       Install Common Utilities
                     │
                     ▼
         Ready to Install Java
                     │
                     ▼
         Ready to Install Maven
                     │
                     ▼
         Ready to Install Docker
                     │
                     ▼
      Ready to Install Kubernetes
                     │
                     ▼
       Ready for DevOps Projects 🚀
```

---

# 🌍 Real World Example

Suppose you join a new company.

Your manager gives you a brand-new Ubuntu VM.

Without installing these utilities

❌ Cannot clone Git repositories

❌ Cannot download installers

❌ Cannot unzip project files

❌ Cannot edit configuration files

❌ Cannot inspect project folders

❌ Cannot securely install software

After installing them

✅ Clone repositories

✅ Download software

✅ Install Java

✅ Install Docker

✅ Install Kubernetes

✅ Configure Jenkins

✅ Work on CI/CD pipelines

---

# 💡 Summary

These utilities are the first tools installed on almost every Linux server used by DevOps Engineers.

| Utility | Purpose |
|----------|----------|
| apt | Install software |
| -y | Automatically answer Yes |
| curl | Call APIs & download scripts |
| wget | Download files |
| Git | Version Control |
| zip | Compress files |
| unzip | Extract ZIP files |
| vim | Edit files |
| jq | Format JSON |
| tree | Show folder structure |
| net-tools | Network troubleshooting |
| ca-certificates | SSL certificates |
| software-properties-common | Add repositories |
| apt-transport-https | Install packages securely over HTTPS |
| gnupg | Verify package authenticity |
| lsb-release | Show Linux version |

---

## 🎉 Congratulations!

If you understand all the utilities explained in this guide, you have built a strong Linux foundation for your DevOps journey.

Next Steps:

- ✅ Java
- ✅ Maven
- ✅ Git
- ✅ Docker
- ✅ Kubernetes
- ✅ Jenkins
- ✅ Terraform
- ✅ Ansible
- ✅ CI/CD Pipelines

Happy Learning! 🚀