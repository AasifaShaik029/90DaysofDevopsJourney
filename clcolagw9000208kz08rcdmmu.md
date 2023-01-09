# Day 6: Getting started with Git and GitHub

Hello Everyone!

This article is going to be very useful for beginners in Git and GitHub. Give it a read. Hope it will be useful 🙂

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1673255772479/1b98cc2a-b901-4624-b6c1-5b63a19784c6.png align="center")

### Centralized Version Control System Vs Distributed Version Control System

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1673082461836/8de27ad0-6a4c-4046-9f48-515f198a1fe7.png align="center")

**Centralized version control system:**

Generally in centralized source control there is a server (remote/repo) and a client (local/PC). The server is the master repository that contains all the code versions.

To work on any project, Client will pull the code from the repo and start working on it into the local machine. Everything is centralized in this model. It means once you get the latest version of the code, you start making your own changes in the code and after that, you simply need to commit those changes into the master repo. So the basic workflow involves in the centralized source control is getting the latest version of the code from a central repository that will contain other people’s code as well, making your own changes in the code, and then committing or merging those changes into the central repository.

Disadvantages of CVCS:

1.Single point of failure

2.Voluntary changes that breaks build entirely

3.Remote commits are slow

**Distributed version control system:**

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1673091533355/5a0e9844-57c7-42a0-8800-9067fd90641f.png align="center")

Instead of one single repository which is the server, here every single developer or client has their own server and they will have a copy of the entire history or version of the code and all of its branches in their local server or machine.

**Advantages:**

Easy to commit changes, developers can easily push their changes into their repos. While pushing to the master repo, integrator manager will review the codes from local repos , only code with noissue will be pushed to master repo and code with issues will be worked on.

---

### **Difference between Git and GitHub**

GIT is a DVCS works on our local machine. GitHub and GitLab are hosted Gits works on cloud/web.

when you start working on a project, you clone the code from the master repository (github) in your own local machine.

Now you need to request the master repo to **"push"** your code to master from local repo. Getting the new change from a repository is called “**pulling**”

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1673254950018/a6936c28-8136-4f04-ad26-cbaf373d45fb.png align="center")

---

### **Stages of Git**

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1673094189556/50d53221-a50d-48c1-a61e-c5f78f399e0c.png align="center")

**Working Directory:**

workspace/working dir (location where you work on the code).

**Staging area:**

preparing our project files for a commit. The staging area is an intermediate step between making changes to files and capturing the snapshots of these updates. It is sometimes also known as Git Index.

After writing code in the working dir. you need to “**add**” to the staging area.

**Local repository:**

Commit/save the files u want to save from SA to local repo.

It is like taking snapshots.

Each commit will be having one **commit id /Hash id** (have 40 digit alphanumeric id). If u want versions of the code you can use the commit id.

When you send from SA to local repo then it will create master branch by default.

---

### **Understanding Git and GitHub in a practical way**

Make sure you have GitHub account. Install git bash.

In git bash,

A repository is like a folder where code and files will be present.

Create a folder with project1 name and inititialize it with **git init.** Empty git repository will be created. It is a hidden file. (use ls -a)

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1673160218555/db7bb958-8b82-4453-96ac-e660fbf2dab5.png align="center")

you need to commit changes to enter into git repository otherwise it will be just part of your local system.

git status - gives status of git repo

git add . - it will move to staging environment(tracked)

git commit -m "added new file" - It will move to git repo

**Note:** When you remove a file in the terminal it will be removed permanently. But when you are using git repo, if you remove a file then it can be restored. Lets see how 😊

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1673178397562/1c35cbe9-2c34-4555-98dc-9381f4901c0e.png align="center")

Here after committing a file, then if you remove it you can restore it using

**git restore projectfile1.txt (or) git checkout projectfile1**

**It should be staged and committed if you want to restore a file or a directory**.

Lets try removing a folder

You cant restore empty folder so add some files into it. Then add and commit and remove. Now restore a folder.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1673179823209/8c2abb7e-ba34-4087-adb8-cb0d9eb59efa.png align="center")

---

### Setting Global username and email for GIT locally

To track who made commits, we can use **git log,** there you can see the author who made what commit

I created a tstfile1.txt under diff username and email so logs of that file will be displayed under that username and email. Previously samplefolder and file1 are committed under diff username.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1673181634249/100832e4-bbef-4569-8a2e-60504ce60bd9.png align="center")

---

### Git Clone

Cloning is the act of making a copy of any target repository.

Lets create a repo in github and from github we need to clone into our local machine.

Lets see 😌

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1673183026448/6b8ced43-865f-4ac4-8071-aef2faf400c2.png align="center")

Now clone this repo URL into local. Use can see contents from remote copied into local.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1673183237864/3b9b92ad-efd7-410b-a25c-c7a6a917c6ae.png align="center")

---

### Git Push

Pushing repo from local to remote

I changed project1 name to devopslocal. Now I will push this folder to remote.

Before that check remote connection in your local. If it is not connected to your remote then add/connect it.

check using

git remote -v (if nothing displays then add it using 👇)

git remote add origin "remoterepourl"

git push origin "branchname"

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1673185425135/9e567ac1-190e-4dac-8cb1-dfc85a65b981.png align="center")

---

**Note:** The difference between clone and fork is that when you clone you are copying (it links )the remote repo into local and .git of local and remote will be in sync. When you push or pull the changes will be seen.

But in the fork, it is also copying the repo but when you fork the repo if you make changes it won't impact the repos one another as .git of your repo and forked repo will be run differently. If you want to sync the repos you need to select sync fork option.

---

### Branching and Merge

Let's say we are working on a project and the initial branch will be created

👉In Local the default branch is Master

👉In remote the default branch is Main

Now if you want to work on a new feature of the project it should be worked under another branch so if it has any issues it should not impact the initial or main branch. So we create a separate branch and work on it. If that feature is working well, it will be **Merged** into the initial/master branch.

"Branches allow you to work on different parts of a project without impacting the main branch."

When you created a new branch then if you make changes to it, then the main branch won't be effected. You need to merge for that.

Below I created a feat/addemojis branch and added a text file in it and staged and committed you can see that it wont be available in master branch.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1673246794238/6ecefc50-8389-48e4-9345-8db3774eeeaa.png align="center")

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1673252794264/eed98083-cab6-4121-9926-58c0ce3b287f.png align="center")

If you want to push the newly created branch to remote then

**git push origin newbranchname**

If you want to merge in local use merge, if you want to merge in remote, use pull request--&gt;request merge request.

\--Thats all about Git basics. Thank you for your time 🙂--