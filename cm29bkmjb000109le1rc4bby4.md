---
title: "Setting up Gitlab in EC2"
datePublished: Mon Oct 14 2024 18:00:08 GMT+0000 (Coordinated Universal Time)
cuid: cm29bkmjb000109le1rc4bby4
slug: setting-up-gitlab-in-ec2
cover: https://cdn.hashnode.com/res/hashnode/image/upload/v1728928748820/88178af3-759e-4d0a-acd0-727d9ac90ba2.png
tags: setting-up-gitlab-in-ec2

---

# Setting up Gitlab in EC2 so that we can push our docker file in gitlab

To avoid being prompted for your GitLab username and password every time you perform a `git push`, `pull`, or `fetch`, you can configure Git to use SSH keys instead of HTTPS authentication. Here are the steps to set it up on your EC2 instance:

### Step 1: Generate an SSH key

If you don’t already have an SSH key, you can generate one:

```plaintext
bashCopy codessh-keygen -t rsa -b 4096 -C "your_email@example.com"
```

* Press enter to accept the default location (`/home/ubuntu/.ssh/id_rsa`).
    
* Optionally, you can set a passphrase, or press enter for no passphrase.
    

### Step 2: Add the SSH key to the SSH agent

Start the SSH agent in the background and add your key:

```plaintext
bashCopy codeeval "$(ssh-agent -s)"
ssh-add ~/.ssh/id_rsa
```

### Step 3: Copy the SSH key to your clipboard

To display your public key, use the following command:

```plaintext
bashCopy codecat ~/.ssh/id_rsa.pub
```

Copy the output of this command to your clipboard.

### Step 4: Add SSH key to GitLab

1. Log in to GitLab.
    
2. Go to **Profile settings** → **SSH Keys**.
    
3. Paste the SSH key from your clipboard into the "Key" field.
    
4. Give the key a title, then click **Add key**.
    

### Step 5: Update your Git remote to use SSH

In your Git repository, switch the remote URL from HTTPS to SSH:

```plaintext
bashCopy codegit remote set-url origin git@gitlab.com:username/repository-name.git
```

### Step 6: Test the SSH connection

You can test your connection to GitLab using:

```plaintext
bashCopy codessh -T git@gitlab.com
```

If everything is set up correctly, you'll see a message like:

```plaintext
cssCopy codeWelcome to GitLab, @username!
```

Now, you won’t be prompted for your GitLab credentials when pushing or pulling changes!

To push your Dockerfile to GitLab, follow these steps:

### 1\. **Ensure Your GitLab Repository is Set Up**

Make sure your GitLab repository is properly initialized and connected. If it's not already done, you can initialize a Git repository in your project and connect it to GitLab:

```plaintext
bashCopy codegit init
git remote add origin git@gitlab.com:<username>/<your-repo-name>.git
```

### 2\. **Add the Dockerfile to Your Project**

If your Dockerfile is already in the project folder, you can skip this. Otherwise, ensure it’s in your project’s root directory.

### 3\. **Check Your Git Status**

Run this to see what files are ready to be added:

```plaintext
bashCopy codegit status
```

### 4\. **Stage the Dockerfile**

Stage the Dockerfile (and any other files you want to push):

```plaintext
bashCopy codegit add Dockerfile
```

You can also add all files:

```plaintext
bashCopy codegit add .
```

### 5\. **Commit the Dockerfile**

Commit the changes with a meaningful message:

```plaintext
bashCopy codegit commit -m "Added Dockerfile for Angular app deployment"
```

### 6\. **Push to GitLab**

Finally, push the changes to GitLab:

```plaintext
bashCopy codegit push origin main
```

If you're using an SSH key, it shouldn't prompt for a username or password. Otherwise, it will ask for authentication unless you've set up SSH keys (which you mentioned you were trying to do).

After this, your Dockerfile and any other changes will be pushed to your GitLab repository.