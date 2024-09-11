---
title: "Automating Frontend App Deployment with Jenkins and Nginx"
datePublished: Wed Sep 11 2024 11:47:10 GMT+0000 (Coordinated Universal Time)
cuid: cm0xspvhd001v08mbdhzrb9s9
slug: automating-frontend-app-deployment-with-jenkins-and-nginx
ogImage: https://cdn.hashnode.com/res/hashnode/image/upload/v1726055186739/ee827829-3c19-46b8-a42d-1b820c50f788.webp
tags: frontend, nginx, jenkins, frontend-deployment

---

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1726031346995/a76de7a5-fdd9-4dad-b971-47be3b927edb.png align="center")

If you're managing a **frontend web app**, Jenkins and Nginx are powerful tools that can help automate deployment and ensure your app is served efficiently. This guide walks you through the steps to deploy your frontend app using Jenkins and Nginx.

* **Jenkins**: A widely-used automation tool for building, testing, and deploying code. With its extensive plugin ecosystem, Jenkins is the perfect tool for automating web app deployments.
    
* **Nginx**: A high-performance web server often used as a reverse proxy. It efficiently serves static content, making it ideal for frontend apps.
    

### **Prerequisites**

Before getting started, make sure you have the following set up:

1. Jenkins installed and running.
    
2. Nginx installed on the server where the app will be deployed.
    
3. A frontend web application hosted on a Git repository (GitHub, Bitbucket, etc.).
    
4. Nginx configuration.
    

### **Jenkins installed and running**

Follow below steps to install and run jenkins on 8080 port of your EC2 instance.

```plaintext
sudo apt update
sudo apt install fontconfig openjdk-17-jre
java -version

sudo wget -O /usr/share/keyrings/jenkins-keyring.asc \
  https://pkg.jenkins.io/debian-stable/jenkins.io-2023.key
echo "deb [signed-by=/usr/share/keyrings/jenkins-keyring.asc]" \
  https://pkg.jenkins.io/debian-stable binary/ | sudo tee \
  /etc/apt/sources.list.d/jenkins.list > /dev/null
sudo apt-get update
sudo apt-get install jenkins
```

you need `sudo visudo` (or appropriate permissions) when copying files from the Jenkins workspace to the Nginx default path (`/var/www/html`) because regular users (including Jenkins) do not have write access to this directory by default.

```plaintext
  sudo visudo
  jenkins ALL=(ALL) NOPASSWD:ALL
```

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1726031849764/235f29fa-81fb-4326-a449-79a4a204c934.png align="center")

Access Jenkins using http://&lt;public-ip&gt;:8080.

### Nginx installed on the server where the app will be deployed

```plaintext
sudo apt-get update
sudo apt install nginx
sudo systemctl enable nginx
```

### Creating Freestyle Job

Give your github repo where your frontend app exists. If it is a private repo, give credentials as well. ( username (username of github) & password (personal access token )).

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1726053783308/6583b626-0ba1-4aee-b0f8-0f523daca4e7.png align="center")

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1726053807728/cd8a0331-5164-49be-b691-d412ad09d9cb.png align="center")

### Setting up webhook trigger

Repo —&gt; settings —&gt; webhook —&gt; give your jenkins url/github-webhook/ ex: [http://3.80.153.218:8080/github-webhook/](http://3.80.153.218:8080/github-webhook/)

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1726053827654/8f17289b-920e-4552-8974-6585e67f4817.png align="center")

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1726053902261/0a9a7a49-cae2-474c-aa39-3e2b20821e53.png align="center")

Test the webhook by modifying or adding files in github. As soon as we modify the repo then jenkins job automatically runs and builds.

### Build Steps

"When we build the Jenkins job, all the frontend files will be moved to the Jenkins workspace. We then need to copy these files to Nginx's default location, where the server looks for files to run the app."

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1726054024937/1c68f4a9-c6de-4eca-adf4-ee36bd1a1df2.png align="center")

When the Jenkins job is executed, all frontend files are stored in the Jenkins workspace directory. The command `sudo cp * /var/www/html/` is used to copy these files from the workspace to Nginx's default location. To avoid permission issues, `sudo` is required since `/var/www/html/` is a privileged directory. Ensure the Jenkins user has appropriate `sudo` privileges for this command to run smoothly. ( Check above for setting privileges in sudo visudo )

* Click on build after adding build steps.
    
* ![](https://cdn.hashnode.com/res/hashnode/image/upload/v1726054906472/e428e85a-ac62-4813-8c79-f768190ba136.png align="center")
    

I made changes to my file. Trigger will automatically starts.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1726054964578/0223f3a5-aaf7-49b0-9321-1493f30e4433.png align="center")

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1726054994401/4db57880-3c00-4ab2-b5cb-5ae1ace4e596.png align="center")

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1726055013498/403e5044-a7f4-4de4-8711-ae9fde47c138.png align="center")

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1726055031208/878f7bf7-e4a6-4a56-97b4-fbea8bea3226.png align="center")

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1726055050316/df713cc4-1913-46e5-8483-07c9d140871e.png align="center")

Our app got succesfully deployed.