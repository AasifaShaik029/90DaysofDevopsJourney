---
title: "Shell Scripting Concepts and Projects"
datePublished: Sat Aug 12 2023 13:44:25 GMT+0000 (Coordinated Universal Time)
cuid: cll82jbi2000009mndyqw567c
slug: shell-scripting-concepts-and-projects
cover: https://cdn.hashnode.com/res/hashnode/image/upload/v1691847817761/a50dcbd3-567d-4228-9397-4a74749f4790.jpeg
tags: devops, shell-scripting, shell-script, bash-shell-script

---

### Why Shell scripting is important and What can we do using Shell scripting?

**Infrastructure as Code (IaC):**

Shell scripts can be used to automate the provisioning and configuration of infrastructure. This includes tasks like setting up virtual machines, containers, networks, and more. With tools like Ansible, Terraform, or cloud provider-specific APIs, you can define your infrastructure as code and use shell scripts to deploy and manage it.

**Git Repo Management:**

Shell scripts can simplify common Git-related tasks like cloning repositories, pulling updates, branching, merging, and pushing changes. This makes code management and collaboration more efficient for development teams.

**Automation of Cron Jobs:**

Shell scripts are commonly used to automate recurring tasks using cron jobs. These tasks can include regular data backups, log file rotation, database maintenance, and other scheduled operations.

**Working with Ansible or other Configuration Management Tools:**

Shell scripts can help in preparing the environment for configuration management tools like Ansible. For instance, you can use shell scripts to install necessary packages, configure network settings, and set up prerequisites for Ansible playbooks.

**Server Health Monitoring:**

Shell scripts can be designed to collect system metrics, monitor resource utilization, check for service availability, and generate alerts when thresholds are breached. This proactive approach helps in identifying and addressing potential issues before they cause significant problems.

**Health Checks for Multiple VMs:**

If you have a large number of virtual machines (VMs), shell scripts can be used to perform health checks across all VMs in an automated manner. This can help you quickly identify any common issues affecting multiple VMs, saving time and effort compared to manual checks.

**Custom Automation:**

Shell scripts allow you to create custom automation tailored to your specific needs. This could involve complex workflows that involve multiple steps, interactions with APIs, conditional logic, and more.

**System Configuration and Setup:**

Shell scripts can be used to set up software configurations, user accounts, permissions, and other system-level settings consistently across different environments.

**Data Processing:**

Shell scripts can be used for text processing, data manipulation, file transformations, and other data-related tasks. This is particularly useful when dealing with log files, CSV files, and other structured or semi-structured data.

**Deployment and Release Management:**

Shell scripts can be part of your deployment pipelines, helping to automate the process of deploying applications, updating configurations, and managing versioning.

### Shell Scripting Concepts and Best Practices

### set -x

```plaintext
#Debug mode
set -x
```

It will name the command which is getting executed. This can be incredibly helpful for debugging scripts, understanding the flow of execution, and identifying errors or unexpected behavior.

Example without set -x:

```plaintext
#!/bin/bash

echo "Hello aasifa"
echo "This is a test."
```

Output

```plaintext
Hello, world!
This is a test.
```

Example with set -x:

```plaintext
set -x
echo "Hello, aasifa"
echo "This is a test."
```

Output

```plaintext
+ echo 'Hello, aasifa'
Hello, world!
+ echo 'This is a test.'
This is a test.
```

### set -e

Exits the script where there is an error in the script. If we don't use this then even though there is error in the script all the remaining lines will be executed. There might be chances of not noticing the lines.

### A drawback of set -e and use of set -o pipefail

It will not error out when there is s a pipe (|). So along with that we need to use **set -o pipefail** as well. Lets see how? 🤔

In this below script, we actually having and error at uerwusomeerror | echo. It should give error, but it wont give if we use only set -e

```plaintext
#!/bin/bash
set -x
echo "hi"
df -h
set -e
errorblablaieh | echo
free
```

In the above script, if we use pipe which is having the error, actually it should stop executing the further commands. But It will execute the further commands even though there is an error in the before line.

output:

```plaintext
+ echo hi
hi
+ df -h
Filesystem      Size  Used Avail Use% Mounted on
/dev/root       7.6G  1.6G  6.0G  21% /
tmpfs           483M     0  483M   0% /dev/shm
tmpfs           194M  832K  193M   1% /run
tmpfs           5.0M     0  5.0M   0% /run/lock
/dev/xvda15     105M  6.1M   99M   6% /boot/efi
tmpfs            97M  4.0K   97M   1% /run/user/1000
+ set -e
+ echo

+ errorblabla
scriptprac.sh: line 6: errorblabla: command not found
+ free
               total        used        free      shared  buff/cache   available
Mem:          988920      232752      353800         836      402368      610492
Swap:              0           0           0
```

If the last command of the pipeline is valid then it set -e wont work, the script will pass. If it is invalid then it will work, the script will fail. Here we can use set -o pipefail to stop executing the scripts even though if there is an error in pipeline.

```plaintext
#!/bin/bash
set -x
echo "hi"
df -h
set -e
set -o pipefail
errorblabla | echo
free
```

```plaintext
 bash scriptprac.sh
+ echo hi
hi
+ df -h
Filesystem      Size  Used Avail Use% Mounted on
/dev/root       7.6G  1.6G  6.0G  21% /
tmpfs           483M     0  483M   0% /dev/shm
tmpfs           194M  832K  193M   1% /run
tmpfs           5.0M     0  5.0M   0% /run/lock
/dev/xvda15     105M  6.1M   99M   6% /boot/efi
tmpfs            97M  4.0K   97M   1% /run/user/1000
+ set -e
+ set -o pipefail
+ echo

+ errorblabla
scriptprac.sh: line 7: errorblabla: command not found
```

It stopped executing further commands

\--&gt; When it comes to executing the pipelines,it will check the authenticity of the last command of pipeline. Here wshiux is invalid and it will throw error and stops execution. If we write errorblabla | echo alone, then it will throw error as well as executes further steps as echo is valid.

```plaintext
#!/bin/bash
set -x
echo "hi"
df -h
set -e
errorblabla | echo | wshiux
free
```

output

```plaintext
+ echo hi
hi
+ df -h
Filesystem      Size  Used Avail Use% Mounted on
/dev/root       7.6G  1.6G  6.0G  21% /
tmpfs           483M     0  483M   0% /dev/shm
tmpfs           194M  832K  193M   1% /run
tmpfs           5.0M     0  5.0M   0% /run/lock
/dev/xvda15     105M  6.1M   99M   6% /boot/efi
tmpfs            97M  4.0K   97M   1% /run/user/1000
+ set -e
+ wshiux
scriptprac.sh: line 6: wshiux: command not found
+ echo
+ errorblabla
scriptprac.sh: line 6: errorblabla: command not found
```

### curl Vs wget

Curl : Curl is used fetch a file and it retrieves the content and typically displays it in the terminal. By default, `curl` outputs the content to the terminal, allowing you to see the response from the server.

Wget : Wget is specifically designed for downloading files from the internet. When you use wget, it retrieves the file and saves it to the local filesystem.

suppose we have one log file and i want to print only error logs of the file.

Example : cat logfile.txt | grep ERROR

It will print the lines which is having error.

The same I want to perform using curl and wget.

URL OF LOGFILE

[sandbox/log/dummylog01122022.log at main · AasifaShaik029/sandbox (](https://github.com/AasifaShaik029/sandbox/blob/main/log/dummylog01122022.log)[github.com](http://github.com)[)](https://github.com/AasifaShaik029/sandbox/blob/main/log/dummylog01122022.log)

You can see it just retrives the logfile content

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1691826926990/e14b0fed-6904-456b-9f60-4beaf4c7c3c4.png align="center")

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1691826939329/258fab03-94b8-4125-9daf-1c5c2ab4d88a.png align="center")

It didn't download the file. Now I can use this URL to fetch my error logs.

```plaintext
curl https://github.com/AasifaShaik029/sandbox/blob/main/log/dummylog01122022.log |  grep ERROR
```

wget

```plaintext
wget https://github.com/AasifaShaik029/sandbox/blob/main/log/dummylog01122022.log |  grep ERROR
```

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1691827066717/60ee56e3-edcc-416b-9a75-433bfb670ef9.png align="center")

It downloaded and now we can use downloaded file to retrieve our error logs.

```plaintext
cat dummylog01122022.log | grep ERROR
```

### Find

The find command is used to search for files and directories in the entire file system/VM.

Example : In linux file system hierarchy there are no of files and directories.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1691833825459/4130aec1-34e2-4bb2-845b-4fb3a7ceb04a.png align="center")

Now I want to find chrony from my entire Vm,

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1691834285864/a9c54cc4-19f2-4701-8911-4508cc03b0d3.png align="center")

```plaintext
sudo find / -name chrony
```

I could see all the different paths where chorny is present as below

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1691834526564/6da9bbef-0c9b-43a5-a9d4-ef36aef7a140.png align="center")

### Search for a specified Directory

If you want to search only directory the use **\-type d**

```plaintext
#searching directory within a specified path
sudo find /home/ubuntu -type d -name mydirectory

#searching directory in the entire vm
sudo find / -type d -name mydirectory
```

### Search for a Specific File Name

Just remove -type d or add -type f

```plaintext
#searching file within a specified path
sudo find /home/ubuntu  -name myfile.txt

#searching file in the entire vm
sudo find / -name myfile.txt

sudo find / -type f -name crony
```

### Search for both file and directory

```plaintext

#searching file also directory with the name chrony  in the entire vm
sudo find / -name chrony
#searching file also directory with the name chrony  in the specified path
sudo find /home/ubuntu  -name chrony
```

### Trap

Trap is used to specify what actions should be taken when a specific signal is received by the script. (signals are interrupts like ctrl c /SIGINT).

By setting up a trap, you can define how your script should respond to these signals.

Syntax

```plaintext
trap action signal
```

Example

```plaintext
#!/bin/bash

# Function to execute when Ctrl+C is pressed
ctrl_c() {
    echo "Ctrl+C Pressed by user $USER! Executing additional actions..."
    exit 1
}

# Set up the trap for Ctrl+C (SIGINT)
trap ctrl_c SIGINT

# Your script logic
echo "Press Ctrl+C to see the message..."
while true; do
    echo "hello aasifa"
    sleep 1
done

```

The above script is, when we press ctrl c the ctrl\_c function will be executed, where it will display the message and exit from the script. Otherwise, it will keep printing hello aasifa continuously after every 1 sec. (you need to mention exit 1 to exit from the script else after printing function statements, it will keep printing hello aasifa.