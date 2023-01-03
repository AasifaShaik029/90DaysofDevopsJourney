# Day 2 : Linux Fundamentals

[Batch II - Advance DevOps - Zero to Hero (Dec'22) - Course (](https://www.trainwithshubham.com/s/courses/634d4d55e4b06642a8ba423d/take#63a6d88fe4b0d591a4f51f24)[trainwithshubham.com](http://trainwithshubham.com)[)](https://www.trainwithshubham.com/s/courses/634d4d55e4b06642a8ba423d/take#63a6d88fe4b0d591a4f51f24)

Today I have gone through Linux Fundamentals which covered the following topics.

1. What is Linux?
    
2. File system hierarchy
    
3. Getting Started with commands
    

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1671960101697/f8ced6a1-60b1-4105-a1c6-89501d109d4b.png align="center")

Just like Windows, iOS, and Mac OS, Linux is an operating system.

**Ex:** Android, is powered by the Linux operating system.

There are so many flavors of Linux.

Ubuntu, Fedora, kali, redhat, centos, etc.

**Shell:** In order to interact with OS we need a shell. The shell can be terminal or bash. (Black window) It is a medium to connect hardware and applications.

It is a command-line interface. Enter a command to execute kernel function.

**Kernel:** The kernel is the core part of the operating system. It is software (code built in C language ) that connects hardware to applications via shell. Virtualise and control computer hardware (CPU, memory etc)

Kernal will be updated twice a year (4 and 10th months)

**System utility:** provides os functionalities.

**File system hierarchy**

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1671962136127/52326b77-105e-4ee7-b0bc-15596bc10a61.png align="center")

/ is the starting point of FSH. Top-level directory.

/root is the superuser who has all the permissions.

/bin: Contains all the binary executables. contains all the Linux commands.

bin is a standard subdirectory of the root directory

/dev: device files.

/etc: local system configurations files

/home: users' subdirectory for storage. The Linux home directory is a directory for a particular user of the system and consists of individual files. It is also referred to as the **login directory**. This is the first place that occurs after logging into a Linux system. It is automatically created as **"/home"** for each user in the directory.

/opy :optinal files -third party tools

/sbin: executables for system administration

/temp:stores temporary files- cleared during system boot

/usr: executables, libraries,man files etc

/var: variable data files --&gt;ex: logs, email inbox, web app related files, cron files etc

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1671978591431/4804825e-57f3-4738-9663-cc03574af52d.png align="center")

$ is a user

~ is a home directory

if you switch to root user that $ will change to #

#is a root user

/home/ubuntu is current directory

**<mark>Commands:</mark>**

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1671962366504/d3cc46f6-209a-4d2f-9d46-a05c12c315c5.png align="center")

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1671962428918/a3daec85-f205-49de-b0d4-45fee58b1220.png align="center")

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1671962471586/000e9e6e-8b78-4fa1-b096-0b18b8a6f154.png align="center")

**<mark>Copying files:</mark>**

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1671963243708/d4a0ede6-62dd-42cc-94d3-22bc2e34f1f7.png align="center")

**<mark>Removing file and directory:</mark>**

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1671963285242/e6361fe9-f39e-4858-9f6e-3913883b508b.png align="center")

**<mark>Permissions:</mark>**

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1671962658208/ae5b65e1-352a-4337-a9ec-79bfa44c91b4.png align="center")

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1671962858106/baa36029-5987-4d1a-958e-486f610cb4be.png align="center")

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1671962918843/f0f80d42-4660-4454-bf71-5a0217c59fae.png align="center")

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1671962977520/7c2b5638-98ee-43fc-a902-ba85883ebd30.png align="center")

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1671963047145/6d874958-7bc2-421c-b44c-ac64b53d9ead.png align="center")

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1671963096671/05028cd6-f801-42c9-8eb3-7778d2771ddf.png align="center")