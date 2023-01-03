# Day 3: Few Advanced Linux Concepts

Hello Everyone!

I have completed my Day 3 learning part. Please find my below Notes. Hope it will be helpful.

**Concepts this Article covers:**

1. Setting up an SSH client
    
2. SCP (Secure Copy)
    
3. User Management
    
4. Group Management
    
5. Linux File System Permissions
    
6. grep (Global regular expression print), find, awk
    

These are the topics that covered as part of Day3 learning. Please find below notes.

⬤ **Setting up SSH client**

SSH is a secure shell. A protocol typically used for connecting to Linux servers. The command-line SSH tool lets you log into your server and run commands remotely to perform any required task.

ssh (SSH client) is a program for logging into a remote machine and for executing commands on a remote machine. It is intended to provide secure encrypted communications between two untrusted hosts over an insecure network.

We can use ssh in windows. For that, it needs to be enabled. 👇

[How to Enable and Use SSH Commands on Windows 10 (](https://winbuzzer.com/2021/08/25/how-to-enable-and-use-ssh-commands-on-windows-10-xcxwbt/)[winbuzzer.com](http://winbuzzer.com)[)](https://winbuzzer.com/2021/08/25/how-to-enable-and-use-ssh-commands-on-windows-10-xcxwbt/)

In AWS Linux, it will be already installed by default.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1672120095620/815a41a6-fb83-47d6-b3a2-516c492be642.png align="center")

⬤ **Connecting through SSH Client in windows CMD:**

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1672202456030/090e41b9-dd99-4686-b19f-b00a60f0c3c4.png align="center")

In windows, go to path where pem file is download and give command mentioned in SSH client page of aws --&gt;ssh -i "NodePair.pem" [ubuntu@ec2-3-110-182-104.ap-south-1.compute.amazonaws.com](mailto:ubuntu@ec2-3-110-182-104.ap-south-1.compute.amazonaws.com) then enter

Here -i is for giving your pem file

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1672504901035/60eb774b-d7fb-4cbe-92a4-0ebc81f2f791.png align="center")

⬤ **SCP: Secure Copy**

“secure copy” allows us to secure transferring of files between [localhost](http://localhost) and remote host or between two remote hosts.

Local host------------&gt;Remote Hosts

sudo scp -i NodePair.pem localfile.txt [ubuntu@ec2-3-110-182-104.ap-south-1.compute.amazonaws.com](mailto:ubuntu@ec2-3-110-182-104.ap-south-1.compute.amazonaws.com) :/home/ubuntu/aasifa

⬤ **User Management:**

Creating users and groups.

#To check username who is currently logged in - whoami

#to add user, you need permissions to add user, -m creates users home directory

**sudo useradd ayman -m**

#to view if user is added

**cat /etc/passwd**

#setting up password for ayman user

**sudo passwd ayman**

#you cant enter into ayman by cd ayman, you either need to switch to ayman or give permissions

**su ayman**

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1672506631243/c29c16c8-f95d-4b28-a27c-c2a5950ce009.png align="center")

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1672506905832/a833a559-7473-473d-b8fc-6821b7e95c0f.png align="center")

**⬤ Group Management**

**Creating a Group:**

#we need few more users to add them to a group

**useradd rosy**

**useradd jasmine**

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1672507314704/d7af13c9-848b-41b2-b5cf-164f7c30853d.png align="center")

#first creat a group and name it then add all the users into it

**sudo groupadd developers**

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1672507517855/25b2671d-a5e2-4c32-af96-22ed4ca1b771.png align="center")

#adding users into developers groups

#here -M overrides the group

**sudo gpasswd -M ayman,rosy,jasmine developers**

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1672507797507/2aac1caa-4213-40ca-a360-73404b10dbc9.png align="center")

#here -M overrides the existing group

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1672508010712/23c064a1-b6f8-45ea-b71f-f0bd979abcef.png align="center")

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1672508020525/fb8648a6-c163-42c2-be3f-7f784509f6b3.png align="center")

#to add a user to a group -a

**sudo gpasswd -a ayman developers**

#Now when we give permission to a group then it applies to all the users inside it.

#to remove a user from the group

**sudo gpasswd -d ayman developers**

#to delete the group

sudo groupdel developers

**⬤ Linux File System Permissions**

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1672556277080/1c757ffc-3a4b-4515-9254-9bb56f1dd636.png align="center")

We can set permission as below

1. **chmod u+r /filename** (here giving read permission to the user for the file)
    

you can also give as

1. **chmod g+rwx/filename**
    
2. #to remove permission
    
    **chmod g-rw/filename**
    

**chown:**

#To change ownership of the directory or a file

**sudo chown jasmine testfile.txt**

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1672558141046/b80e20a8-2566-4905-8a93-8caee2d08980.png align="center")

1. We can set permission using numeric values also
    

r(read) - 4

w(write) - 2

x(execute) - 1

#giving rwx permissions to user, rx to group, x to others

**sudo chmod 750 testfile.txt**

#Other is every one that is not the owner or in the group.

For example, if you have a file that is root: root then the root is the owner, users/processes in the root group have group permissions, and you are treated as other.

**⬤ ACL : (Access Control List)**

#To work with ACL you need to install it

**sudo apt install acl**

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1672559236846/a8c95aba-6d4d-4a3b-92f8-bc1bc39f9e1e.png align="center")

[Hadoop - File Permission and ACL(Access Control List) - GeeksforGeeks](https://www.geeksforgeeks.org/hadoop-file-permission-and-aclaccess-control-list/)

permissions only for the owner, one group, and others for any share point or folder or file. ACLs allow you to grant permissions to multiple individuals or groups on a shared item.

[Unlike the basic and regular way of giving permissions to one user that is the owner of a file and one group that is the group owner of a file using the “chmod”](https://tekneed.com/how-to-set-permission-in-linux-and-manage-ownership/) command, if you have to give additional permissions to another user or another group on a file without making the user a member of the group, you will have to use ACL to do it.

When permission is set on a file or directory using ACL, it displays a “+” sign when a list command is used.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1672567311520/72a081f8-0f24-4278-b535-962e948bc6d6.png align="center")

Lets say I have 3 users ayman, rosy and jasmine and a group developers

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1672567461833/e5b9c8f2-4a38-47f7-ae24-95fc9aaa4d0f.png align="center")

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1672567508987/ca482e3a-1be3-47fb-9014-5ac6c517103d.png align="center")

#checking if permissions applied to the folders,users and group using getfacl using ACL

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1672567891621/e1bdf29e-10ce-4cb4-9370-9b16ee8a9f43.png align="center")

Lets create a directory and add users into it

#we added users in acldir

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1672568917469/c213be0c-b8ae-4c38-8174-69400b9b1c66.png align="center")

#first check permission in acldir

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1672568601817/9282c144-2a90-4488-804c-cb64251a8884.png align="center")

#Now give permissions to acluser1 which was created under acldir

#set read and execute permission to the acluser1 which is a user present in acldir

**setfacl -m u:acluser1:rx acldir**

#check if permissions applied or not

**getfacl acldir**

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1672569401178/2cee4029-c664-4d3b-85c1-7e625cf7b495.png align="center")

#Now create a group " aclgrp " and add all these aclusers1,2,3 into it

#giving rwx permission to aclgrp

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1672570409811/31e6a1cd-b617-4a4f-a7a2-5e507a8b775f.png align="center")

**⬤ grep (global regular expression print)**

Here regular expression is used to search data, matching complex patterns

#Install grep if you don't have

**sudo apt-get install grep**

You can search in a particular folder or a file

Examples:

**grep devops /home/ubuntu -- displays devops word in ubuntu folder**

**grep INFO test.txt**

#search the string in output of first command

**cat sample.txt | grep \[options\] string**

```plaintext
Options Description
-c : This prints only a count of the lines that match a pattern
-h : Display the matched lines, but do not display the filenames.
-i : Ignores, case for matching
-l : Displays list of a filenames only.
-n : Display the matched lines and their line numbers.
-v : This prints out all the lines that do not matches the pattern
-e exp : Specifies expression with this option. Can use multiple times.
-f file : Takes patterns from file, one per line.
-E : Treats pattern as an extended regular expression (ERE)
-w : Match whole word
-o : Print only the matched parts of a matching line,
 with each such part on a separate output line.

-A n : Prints searched line and nlines after the result.
-B n : Prints searched line and n line before the result.
-C n : Prints searched line and n lines after before the result.
```

Interview Question:

To know if user is created or not use

**sudo grep ayman /etc/passwd**

**Find:**

The Major difference is **FIND** is for searching files and directories using filters while **GREP** is for searching a pattern inside a file or searching process(es).

Find can perform,

searching by file, folder, name, creation date, modification date, owner and permissions.

Syntax:

**find \[where to start searching from\] \[-options\] \[what to find\]**

examples:

#it will find(display) name of the file called example.txt in the current dir

**find . -name example.txt**

#it will find all the files which are .jpg files in the home dir

**find /home -name \*.jpg**

```plaintext
-exec CMD: The file being searched which meets the above criteria and returns 0 for as its exit status for successful command execution.
-ok CMD : It works same as -exec except the user is prompted first.
-inum N : Search for files with inode number ‘N’.
-links N : Search for files with ‘N’ links.
-name demo : Search for files that are specified by ‘demo’.
-newer file : Search for files that were modified/created after ‘file’.
-perm octal : Search for the file if permission is ‘octal’.
-print : Display the path name of the files found by using the rest of the criteria.
-empty : Search for empty files and directories.
-size +N/-N : Search for files of ‘N’ blocks; ‘N’ followed by ‘c’can be used to measure size in characters; ‘+N’ means size > ‘N’ blocks and ‘-N’ means size < ‘N’ blocks.
-user name : Search for files owned by user name or ID ‘name’.
\(expr \) : True if ‘expr’ is true; used for grouping criteria combined with OR or AND.
! expr : True if ‘expr’ is false.
```

**awk command:**

[Top 20 AWK Command in UNIX/LINUX with Examples \[Updated\] (](https://hackr.io/blog/awk-command-unix-linux-examples)[hackr.io](http://hackr.io)[)](https://hackr.io/blog/awk-command-unix-linux-examples)