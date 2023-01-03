# Shell Scripting Mini Project

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1672646431949/b54c0e53-4b3a-4d1c-8f34-73751bb1098e.png align="center")

Hi Everyone! I have completed a small task on shell scripting which gives knowledge of a few commands. Please find the task below :)

**Shell Script based on the below criteria**

✦ Welcome the user, with greetings to his/her username  
✦ Show the date and time   
✦ Show the uptime of the server and the last logins  
✦ Show the disk space and RAM utilization  
✦ Show the top CPU processes  
✦ Add more beautification and commands if you know them

#To display the current user we use the $**whoami** command

#command which shows the date and time **$date**

#uptime is used to find out how long the system is active (running) **$uptime**

#to find last login details **$last** if we want first no.of details then we can use **$head**

#memory command which gives details about disk space is **$df -H**

#memory command which gives details about RAM is **$free**

**Disk Space:** Out of the overall size of 8.2G(8th arg) used is 2.1G(9th arg)and available is 6.2G(10th arg)

#The available disk space is $10 / $8

**df -H | xargs | awk '{ print $10 "/" $8 }'**

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1672633170675/698ce555-fefd-4ec6-9262-426d082e22e5.png align="center")

**CPU Utilization: free**

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1672634174118/39f738ce-20de-44b5-9714-3bc76fccec69.png align="center")

#we need available /total ($13/$8)

**free | xargs |awk '{print $13 "/" $8}'**

#Show the top CPU processes, we use **$top** command

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1672635061305/5d24536d-f48c-406d-bb05-3dc9c958d699.png align="center")

**Script:**

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1672645864955/a151d71a-8bb2-4a3f-98a7-f010895b3ddf.png align="center")

**output:**

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1672645845434/203a7cc5-6eda-49fd-b792-6aacba997e1e.png align="center")