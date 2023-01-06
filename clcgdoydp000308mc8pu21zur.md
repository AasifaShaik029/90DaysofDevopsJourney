# Day 4: Introduction to  Shell Scripting

Hello Everyone!

This is Day 4 learning. It is a very Beginner friendly Note. Give it a read :)

Please find below the concepts this article covers on Shell Scripting.

1. Few Basic Commands
    
2. Checking file and directory exists or not
    
3. Writing a script
    
4. Comments
    
5. Arguments
    
6. File Creation
    

7\. if, elif, and for loop

---

#To check which flavor of Linux you are using

**cat /etc/os-release**

#To know which bash you are using

**echo $SHELL or echo $BASH**

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1672676959870/7f37ca92-c42c-474b-bfdb-8c4d735557f3.png align="center")

#who are the people who logged in

**$who**

**$who -H** (-H displays header)

#Checking if a file exists or not

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1672719830085/0e354f07-0a22-41c6-99a0-b3ad138f13b9.png align="center")

The ‘-f’ option in brackets tells the command to check whether the specified file exists.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1672719802705/48c84f8d-64ab-403f-beb8-f8c2d33e78f3.png align="center")

#Checking if a directory exists or not

#$1 in the script represent the command line argument that we will pass while ging execute command.

**#!/bin/bash**

**dir=$1 if \[ -d $1 \]**

**then echo "$1 directory exists"**

**else**

**echo "Sorry $1 directory doesnt exists"**

**fi**

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1672724383600/b189a446-6840-481c-8c18-f3ce0637099c.png align="center")

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1672724329192/403cbf18-a8c7-4e02-bca3-b94e7f57c08e.png align="center")

---

**Writing a Script:**

**Editors to write the script:**

There are different editors where we can write our script. Ex: vim, vi, nano, pico etc

* Vim is a mode-based text editor while Nano is modeless. Mode-based editor means that you need to enter INSERT mode before you can write text to the file.
    
* Nano is an improvement over the Pico text editor, whereas Vim is an improved version of the Vi editor
    

We will use vim editor.

#create a file called sample.sh, .sh represents it is a shell script

**vim sample.sh**

**Executing a file:**

we can execute in 2 ways

1. bash sample.sh
    
2. ./ sample.sh
    

In the second method, we need to assign execute permission to the file

**chmod 777 sample.sh**

**ls -l**

this file becomes executable and will be in green color

./sample.sh

**Inside vim:**

Press i key to insert

Esc :wq to save

**Getting Started with Script:**

#!/bin/bash

\# = sharp

! = bang

#! = shebang

---

**Comments:**

single line comment - #

Multiline Comment -

&lt;&lt; 'COMMENT'

\--

\--

COMMENT

---

**Arguments:**

$1 represents command line argument, when you use $1 in the script it mean you need to give argument after filename. $2 - two arguments, $3 - three arguments.

$# - count of arguments

$$-PID of the script

$\*-Represent all the arguments as a single string

$@ - Same as $∗, but differ when enclosed in (")

$?-Represent last return code

**Example:**

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1672748959735/b98307c2-d9ff-438b-a11e-aef848ef71e3.png align="center")

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1672748923479/cb8b8559-9121-449e-891a-97f0ab673044.png align="center")

---

**Sample file creation :**

We will create a directory - (mkdir)

move to that directory - (cd directory name )

create a file in that dir - touch filename

Give data into it - echo "" &gt; filename

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1672757694335/bbcbc74a-ab70-4173-be71-eaea19b3f948.png align="center")

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1672757642422/86040492-b81a-4fa0-9573-3c039c57788b.png align="center")

---

**if statement:**

**Note:** Linux is space sensitive.

There should be space after "if" and after "\[\[" and before "\]\]"

n=1

if \[ $n -eq 1 \]

then echo "This is true" fi

**elif statement:**

#!/bin/sh

a=10 b=20

if \[ $a == $b \]

then

echo "a is equal to b"

elif \[ $a -gt $b \]

then

echo "a is greater than b"

elif \[ $a -lt $b \]

then

echo "a is less than b"

else

echo "None of the condition met"

fi

**for loop:**

#!/bin/bash

echo "Lines from $1"

for (( i=1; i&lt;=$1; i++ ))

do

echo "$i"

done