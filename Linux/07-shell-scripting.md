# Shell Scripting

This guide explains everything you need to know about Shell Scripting.

If you've ever wondered:

- What is Shell Scripting?
- What is Bash?
- Why is Shell Scripting important?
- How do Shell Scripts work?
- How do you create your first Shell Script?
- How are Shell Scripts used in DevOps?

This guide is for you.

---

# 📑 Table of Contents

1. What is Shell Scripting?
2. What is Bash?
3. Why Learn Shell Scripting?
4. Structure of a Shell Script
5. Variables
6. User Input
7. Common Interview Questions

---

# 1. What is Shell Scripting?

## Answer

A **Shell Script** is a text file containing a sequence of Linux commands that are executed automatically by a shell interpreter.

Instead of typing commands one by one, you can place them inside a script and execute them together.

Shell scripting is widely used for:

- Automation
- System Administration
- DevOps
- Application Deployment
- Backup Operations
- Monitoring
- CI/CD Pipelines

---

## Architecture

```text
User

↓

Shell Script (.sh)

↓

Bash Shell

↓

Linux Kernel

↓

System Resources
```

---

## Key Characteristics

Shell scripts are:

- Easy to Write
- Lightweight
- Powerful
- Portable
- Ideal for Automation

---

## Real World Example

A DevOps engineer creates a shell script that:

- Pulls the latest code
- Builds a Docker image
- Pushes it to a registry
- Deploys it to Kubernetes

Instead of executing dozens of commands manually, a single script performs the entire deployment.

---

## Benefits

Shell scripting provides:

- Automation
- Time Savings
- Reduced Human Error
- Repeatable Tasks
- Faster Deployments

---

## Production Tip

Automate repetitive tasks with shell scripts instead of executing commands manually.

---

## Interview Question

### Question

What is Shell Scripting?

### Answer

Shell Scripting is the process of writing commands in a text file that are executed automatically by a shell interpreter.

---

# 2. What is Bash?

## Answer

**Bash (Bourne Again SHell)** is the default shell for most Linux distributions.

It interprets commands entered by users or stored inside shell scripts.

Bash provides:

- Command Execution
- Variables
- Loops
- Conditions
- Functions
- Automation

---

## Workflow

```text
User

↓

Bash

↓

Linux Kernel

↓

Hardware
```

---

## Check Current Shell

```bash
echo $SHELL
```

Example Output

```text
/bin/bash
```

---

## Check Bash Version

```bash
bash --version
```

---

## Production Example

A CI/CD pipeline executes Bash scripts to build applications, run tests, and deploy software.

---

## Production Tip

Bash is the most commonly used shell in Linux environments, making it an essential skill for DevOps engineers.

---

## Interview Question

### Question

What is Bash?

### Answer

Bash is a command-line shell and scripting language used to execute commands and automate tasks on Linux systems.

---

# 3. Why Learn Shell Scripting?

## Answer

Shell scripting is one of the most valuable skills for Linux and DevOps professionals.

It enables automation of repetitive administrative tasks.

---

## Common Use Cases

- Server Provisioning
- Backup Automation
- Log Rotation
- User Management
- Monitoring
- Deployment Automation
- Health Checks

---

## Automation Workflow

```text
Manual Tasks

↓

Shell Script

↓

Automation

↓

Time Saved
```

---

## Production Example

Every night at 2:00 AM, a shell script:

- Creates database backups
- Compresses log files
- Uploads backups to cloud storage
- Sends a success notification

without requiring manual intervention.

---

## Production Tip

If you find yourself repeating the same commands regularly, consider automating them with a shell script.

---

## Interview Question

### Question

Why is Shell Scripting important?

### Answer

Shell scripting automates repetitive tasks, improves consistency, reduces human error, and increases productivity.

---

# 4. Structure of a Shell Script

## Answer

Every shell script follows a basic structure.

---

## Example Script

```bash
#!/bin/bash

# This is a comment

echo "Hello World!"
```

---

## Components

### Shebang

```bash
#!/bin/bash
```

Specifies which interpreter should execute the script.

---

### Comments

```bash
# This is a comment
```

Comments improve readability and documentation.

---

### Commands

```bash
echo "Hello World!"
```

Commands perform the required operations.

---

## Make Script Executable

```bash
chmod +x script.sh
```

---

## Run the Script

```bash
./script.sh
```

or

```bash
bash script.sh
```

---

## Workflow

```text
Create Script

↓

Make Executable

↓

Execute Script

↓

Output
```

---

## Production Example

A deployment script starts with the Bash shebang and contains comments explaining each deployment step.

---

## Production Tip

Always include a shebang (`#!/bin/bash`) and meaningful comments in production scripts.

---

## Interview Question

### Question

What is the purpose of the shebang (`#!/bin/bash`)?

### Answer

The shebang tells the operating system which interpreter should execute the script.

---

# 5. Variables

## Answer

Variables store data that can be reused throughout a script.

---

## Declare a Variable

```bash
name="Nagarjuna"
```

---

## Access a Variable

```bash
echo $name
```

Output

```text
Nagarjuna
```

---

## Multiple Variables

```bash
server="prod-server"

port=8080

environment="production"
```

---

## Workflow

```text
Variable

↓

Store Value

↓

Reuse Value

↓

Output
```

---

## Production Example

A deployment script stores:

```bash
APP_NAME="shopping-api"

IMAGE_VERSION="v1.2.0"
```

making updates easier without changing multiple commands.

---

## Production Tip

Use meaningful variable names to improve readability and maintainability.

---

## Interview Question

### Question

How do you declare a variable in Bash?

### Answer

Variables are declared using the syntax:

```bash
name="value"
```

without spaces around the equals sign.

---

# 6. User Input

## Answer

Shell scripts can accept input from users during execution.

---

## Example

```bash
echo "Enter your name:"

read name

echo "Welcome $name"
```

---

## Example Output

```text
Enter your name:

Nagarjuna

Welcome Nagarjuna
```

---

## Workflow

```text
User

↓

Input

↓

Variable

↓

Script Processing

↓

Output
```

---

## Production Example

A backup script prompts the administrator for the directory to back up before starting the backup process.

---

## Production Tip

Validate user input before using it in production scripts to prevent unexpected behavior.

---

## Interview Question

### Question

Which command is used to accept user input in Bash?

### Answer

The `read` command accepts input from the user and stores it in a variable.

---

## 📌 Quick Revision

| Topic | Key Concept |
|--------|-------------|
| Shell Script | A file containing Linux commands |
| Bash | Default Linux shell |
| Shebang | `#!/bin/bash` |
| Comments | Start with `#` |
| Variables | Store reusable values |
| `echo` | Display output |
| `read` | Accept user input |
| `chmod +x` | Make a script executable |

---

# 7. Bash Operators

## Answer

Operators allow shell scripts to perform calculations, comparisons, and logical operations.

They are commonly used in conditions and automation workflows.

---

## Arithmetic Operators

| Operator | Description |
|----------|-------------|
| `+` | Addition |
| `-` | Subtraction |
| `*` | Multiplication |
| `/` | Division |
| `%` | Modulus |

---

## Example

```bash
#!/bin/bash

a=20
b=10

echo $((a+b))
echo $((a-b))
echo $((a*b))
echo $((a/b))
```

Output

```text
30
10
200
2
```

---

## Comparison Operators

| Operator | Meaning |
|----------|---------|
| `-eq` | Equal |
| `-ne` | Not Equal |
| `-gt` | Greater Than |
| `-lt` | Less Than |
| `-ge` | Greater Than or Equal |
| `-le` | Less Than or Equal |

---

## String Operators

| Operator | Meaning |
|----------|---------|
| `=` | Equal |
| `!=` | Not Equal |
| `-z` | Empty String |
| `-n` | Non-empty String |

---

## Production Example

A deployment script compares the current application version with the latest version before deciding whether to deploy.

---

## Production Tip

Use arithmetic expansion:

```bash
$((expression))
```

instead of older syntax for better readability.

---

## Interview Question

### Question

What are Bash operators?

### Answer

Bash operators perform arithmetic, comparison, and logical operations inside shell scripts.

---

# 8. Conditional Statements (if, elif, else)

## Answer

Conditional statements allow scripts to make decisions based on specified conditions.

---

## Basic Syntax

```bash
if [ condition ]
then
    commands
fi
```

---

## Example

```bash
#!/bin/bash

age=20

if [ $age -ge 18 ]
then
    echo "Eligible to vote"
fi
```

---

## if...else Example

```bash
#!/bin/bash

number=5

if [ $number -gt 10 ]
then
    echo "Greater than 10"
else
    echo "Less than or equal to 10"
fi
```

---

## if...elif...else Example

```bash
#!/bin/bash

marks=85

if [ $marks -ge 90 ]
then
    echo "Grade A"
elif [ $marks -ge 75 ]
then
    echo "Grade B"
else
    echo "Grade C"
fi
```

---

## Workflow

```text
Condition

↓

True?

↓

Yes → Execute Block

↓

No → Next Condition

↓

Else Block
```

---

## Production Example

A deployment script checks whether Docker is installed before starting a deployment.

```bash
if command -v docker >/dev/null
then
    echo "Docker Installed"
else
    echo "Install Docker First"
fi
```

---

## Production Tip

Always handle unexpected conditions using an `else` block to improve script reliability.

---

## Interview Question

### Question

Why are conditional statements used in shell scripting?

### Answer

Conditional statements allow scripts to make decisions and execute different commands based on specified conditions.

---

# 9. Case Statement

## Answer

The `case` statement provides a cleaner alternative to multiple `if...elif` conditions when comparing a single variable against multiple values.

---

## Syntax

```bash
case $variable in

value1)
    commands
    ;;

value2)
    commands
    ;;

*)
    default commands
    ;;

esac
```

---

## Example

```bash
#!/bin/bash

read -p "Enter environment: " env

case $env in

dev)
    echo "Development Environment"
    ;;

test)
    echo "Testing Environment"
    ;;

prod)
    echo "Production Environment"
    ;;

*)
    echo "Unknown Environment"
    ;;

esac
```

---

## Workflow

```text
Input

↓

case

↓

Matching Pattern

↓

Execute Commands
```

---

## Production Example

A deployment script selects different Kubernetes namespaces based on the chosen environment.

---

## Production Tip

Use `case` statements instead of long `if...elif` chains when evaluating multiple known options.

---

## Interview Question

### Question

When should you use a `case` statement?

### Answer

Use a `case` statement when comparing one variable against multiple possible values.

---

# 10. Loops

## Answer

Loops repeat commands automatically, eliminating repetitive manual work.

---

# for Loop

## Syntax

```bash
for item in list
do
    commands
done
```

---

## Example

```bash
#!/bin/bash

for server in server1 server2 server3
do
    echo $server
done
```

---

## Output

```text
server1

server2

server3
```

---

# while Loop

## Syntax

```bash
while [ condition ]
do
    commands
done
```

---

## Example

```bash
#!/bin/bash

count=1

while [ $count -le 5 ]
do
    echo $count
    ((count++))
done
```

---

## until Loop

Runs until the condition becomes true.

```bash
#!/bin/bash

count=1

until [ $count -gt 5 ]
do
    echo $count
    ((count++))
done
```

---

## Workflow

```text
Loop

↓

Condition

↓

Execute Commands

↓

Repeat
```

---

## Production Example

A monitoring script checks the health of multiple servers every few minutes using a `for` loop.

---

## Production Tip

Ensure loop exit conditions are correct to avoid infinite loops.

---

## Interview Question

### Question

What are the common loop types in Bash?

### Answer

The three common loop types are `for`, `while`, and `until`.

---

# 11. Real-World Automation Examples

## Answer

Shell scripting is widely used for automating system administration and DevOps tasks.

---

## Example 1 – Check Disk Usage

```bash
#!/bin/bash

df -h
```

---

## Example 2 – Display Current Date

```bash
#!/bin/bash

date
```

---

## Example 3 – Backup Directory

```bash
#!/bin/bash

tar -czf backup.tar.gz /home/project
```

---

## Example 4 – Check Service Status

```bash
#!/bin/bash

systemctl status nginx
```

---

## Example 5 – Verify Website Availability

```bash
#!/bin/bash

curl -I https://example.com
```

---

## Production Example

A nightly automation script:

- Checks disk space
- Rotates logs
- Backs up databases
- Verifies application health
- Sends email notifications

all without manual intervention.

---

## Production Tip

Break large automation scripts into reusable functions to improve readability and simplify maintenance.

---

## Interview Question

### Question

What are common real-world uses of Shell Scripting?

### Answer

Shell scripting is commonly used for automation, backups, deployments, monitoring, user management, log rotation, and system maintenance.

---

## 📌 Quick Revision

| Topic | Key Concept |
|--------|-------------|
| Operators | Arithmetic, comparison, string operations |
| `if` | Execute commands based on a condition |
| `elif` | Check additional conditions |
| `else` | Default execution block |
| `case` | Handle multiple possible values |
| `for` | Iterate over a list |
| `while` | Repeat while a condition is true |
| `until` | Repeat until a condition becomes true |

---

# 12. Functions

## Answer

A **Function** is a reusable block of code that performs a specific task.

Instead of writing the same commands multiple times, you define them once and call the function whenever needed.

Functions improve:

- Readability
- Reusability
- Maintainability
- Modularity

---

## Syntax

```bash
function_name() {
    commands
}
```

---

## Example

```bash
#!/bin/bash

greet() {
    echo "Welcome to Linux Shell Scripting!"
}

greet
```

Output

```text
Welcome to Linux Shell Scripting!
```

---

## Function with Parameters

```bash
#!/bin/bash

greet() {
    echo "Hello $1"
}

greet Nagarjuna
```

Output

```text
Hello Nagarjuna
```

---

## Workflow

```text
Function Defined

↓

Function Called

↓

Commands Executed

↓

Return
```

---

## Production Example

A deployment script contains reusable functions such as:

- `build_application()`
- `run_tests()`
- `deploy_application()`
- `rollback_application()`

making the script easier to maintain.

---

## Production Tip

Break large scripts into small, reusable functions with descriptive names.

---

## Interview Question

### Question

Why are functions used in shell scripting?

### Answer

Functions group reusable commands together, making scripts easier to read, maintain, and reuse.

---

# 13. Arrays

## Answer

Arrays allow multiple values to be stored in a single variable.

They are useful for processing lists such as:

- Servers
- Users
- Files
- Directories

---

## Declare an Array

```bash
servers=("web1" "web2" "web3")
```

---

## Access Elements

First element

```bash
echo ${servers[0]}
```

All elements

```bash
echo ${servers[@]}
```

---

## Iterate Through an Array

```bash
for server in "${servers[@]}"
do
    echo $server
done
```

---

## Workflow

```text
Array

↓

Store Values

↓

Loop

↓

Process Each Item
```

---

## Production Example

A monitoring script checks the health of multiple production servers stored in an array.

---

## Production Tip

Use arrays instead of repeating similar variables when handling multiple related values.

---

## Interview Question

### Question

What is an array in Bash?

### Answer

An array stores multiple values in a single variable and allows access using indexes.

---

# 14. File Handling

## Answer

Shell scripts frequently create, read, update, and delete files.

---

## Create a File

```bash
touch file.txt
```

---

## Write to a File

```bash
echo "Hello Linux" > file.txt
```

Append to a file

```bash
echo "New Line" >> file.txt
```

---

## Read a File

```bash
cat file.txt
```

---

## Read Line by Line

```bash
while read line
do
    echo $line
done < file.txt
```

---

## Workflow

```text
File

↓

Read / Write

↓

Process Data

↓

Save Changes
```

---

## Production Example

A shell script reads a list of server names from a file and deploys an application to each server automatically.

---

## Production Tip

Always verify that a file exists before attempting to read it.

Example:

```bash
if [ -f file.txt ]
then
    echo "File exists"
fi
```

---

## Interview Question

### Question

How do you check whether a file exists in Bash?

### Answer

Use:

```bash
[ -f filename ]
```

to verify whether a regular file exists.

---

# 15. Error Handling

## Answer

Error handling makes shell scripts more reliable by detecting failures and responding appropriately.

---

## Check Exit Status

Every Linux command returns an exit status.

```text
0 → Success

Non-zero → Error
```

---

## Example

```bash
mkdir project

echo $?
```

---

## Using if

```bash
if mkdir project
then
    echo "Directory Created"
else
    echo "Failed"
fi
```

---

## Exit on Error

```bash
set -e
```

This stops script execution immediately when a command fails.

---

## Workflow

```text
Execute Command

↓

Check Exit Status

↓

Success / Failure

↓

Continue or Exit
```

---

## Production Example

A deployment script immediately exits if the Docker image build fails, preventing deployment of a broken application.

---

## Production Tip

Use `set -e` in production scripts to avoid continuing after unexpected failures.

---

## Interview Question

### Question

What does `set -e` do?

### Answer

`set -e` causes the shell script to terminate immediately if any command exits with a non-zero status.

---

# 16. Common Beginner Mistakes

## Mistake 1

❌ Forgetting the shebang.

Always start scripts with:

```bash
#!/bin/bash
```

---

## Mistake 2

❌ Not making scripts executable.

Use:

```bash
chmod +x script.sh
```

---

## Mistake 3

❌ Using hardcoded values.

Prefer variables:

```bash
APP_NAME="inventory-service"
```

instead of repeating literal values.

---

## Mistake 4

❌ Ignoring error handling.

Always check command results or use:

```bash
set -e
```

---

## Mistake 5

❌ Writing one very large script.

Split scripts into reusable functions.

---

## Production Tip

Test scripts in a development environment before executing them in production.

---

## Interview Question

### Question

What are common mistakes in shell scripting?

### Answer

Common mistakes include missing the shebang, not handling errors, hardcoding values, forgetting execution permissions, and creating overly large scripts.

---

# 17. Shell Scripting Best Practices

## Answer

Following best practices improves script quality, reliability, and maintainability.

---

## Readability

✅ Use meaningful variable and function names.

---

## Comments

✅ Document complex logic with comments.

---

## Error Handling

✅ Validate inputs and check command success.

---

## Reusability

✅ Use functions for repeated logic.

---

## Security

✅ Quote variables to avoid word splitting.

Example

```bash
echo "$filename"
```

---

## Testing

✅ Test scripts before production deployment.

---

## Production Example

A production deployment script:

- Uses functions
- Validates inputs
- Logs actions
- Handles errors
- Sends notifications
- Exits safely if failures occur

---

## Production Tip

Store reusable shell scripts in version control systems like Git and review changes through pull requests.

---

## Interview Question

### Question

What are shell scripting best practices?

### Answer

Use meaningful names, modular functions, input validation, proper error handling, comments, version control, and thorough testing.

---

# 📌 Quick Revision

### Shell Script Workflow

```text
User

↓

Shell Script

↓

Bash

↓

Linux Kernel

↓

Execution
```

---

### Script Structure

```text
Shebang

↓

Variables

↓

Functions

↓

Conditions

↓

Loops

↓

Execution
```

---

### Important Commands

```bash
bash

chmod +x

echo

read

if

case

for

while

function

set -e
```

---

# Summary

After completing this guide, you should understand:

✅ Shell Scripting Fundamentals

✅ Bash

✅ Script Structure

✅ Variables

✅ User Input

✅ Operators

✅ Conditional Statements

✅ Case Statements

✅ Loops

✅ Functions

✅ Arrays

✅ File Handling

✅ Error Handling

✅ Shell Scripting Best Practices

---

# Interview Quick Revision

| Question | One-Line Answer |
|----------|-----------------|
| What is Shell Scripting? | Writing Linux commands in a file for automated execution |
| What is Bash? | The default command-line shell and scripting language for many Linux distributions |
| What is the purpose of `#!/bin/bash`? | Specifies the Bash interpreter for the script |
| How do you declare a variable? | `name="value"` |
| Which command accepts user input? | `read` |
| What is the purpose of an `if` statement? | Executes commands based on conditions |
| What are common loop types? | `for`, `while`, and `until` |
| Why are functions used? | To organize reusable blocks of code |
| What does `set -e` do? | Stops the script when a command fails |
| How do you make a script executable? | `chmod +x script.sh` |

---

# 🎉 Shell Scripting Module Completed

Congratulations! You have completed **Shell Scripting**, including:

- ✅ Shell Scripting Fundamentals
- ✅ Bash
- ✅ Script Structure
- ✅ Variables
- ✅ User Input
- ✅ Operators
- ✅ Conditional Statements
- ✅ Case Statements
- ✅ Loops
- ✅ Functions
- ✅ Arrays
- ✅ File Handling
- ✅ Error Handling
- ✅ Shell Scripting Best Practices

You now have a strong foundation in Bash scripting and automation—an essential skill for Linux Administration, DevOps, Cloud Engineering, CI/CD, Infrastructure Automation, and Site Reliability Engineering (SRE).

---

# 🚀 What's Next?

➡️ **08-linux-security.md**

In the next guide, you'll learn:

- Linux Security Fundamentals
- Authentication & Authorization
- User & Group Security
- SSH Security
- Firewalls (`iptables`, `firewalld`, `ufw`)
- SELinux & AppArmor
- File & Directory Security
- System Hardening
- Security Monitoring
- Production Best Practices
- Common Interview Questions