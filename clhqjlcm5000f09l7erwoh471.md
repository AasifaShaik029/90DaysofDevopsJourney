---
title: "Deploying a TypeScript App with Serve and Nginx: A Step-by-Step Guide"
datePublished: Tue May 16 2023 17:22:55 GMT+0000 (Coordinated Universal Time)
cuid: clhqjlcm5000f09l7erwoh471
slug: deploying-a-typescript-app-with-serve-and-nginx-a-step-by-step-guide
cover: https://cdn.hashnode.com/res/hashnode/image/upload/v1684257736352/4aa88c91-d9a9-4898-a72e-ad9465200a93.jpeg
tags: aws, nginx, devops, serve

---

Hello Everyone! I hope you are doing great 🙂. I will show you how to deploy a simple typescript calculator app using serve and Nginx. Let's get started.

### Step 1 : Launch an EC2 instance and connect to it using SSH or using Instance Connect.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1684255494624/a19e9350-b5c0-4fa6-aec1-dbcb8eb4e0c6.png align="center")

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1684255576784/5a390d73-a362-48af-aa1b-97176a4185ab.png align="center")

### Step 2: Clone the repository which is having our typescript app called 'Calculator'

Github Repo: [AasifaShaik029/calculator-typescript: This is a Calculator app (](https://github.com/AasifaShaik029/calculator-typescript)[github.com](http://github.com)[)](https://github.com/AasifaShaik029/calculator-typescript)

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1684255739931/d071c9cc-e7e7-4057-8d8c-e3d582522865.png align="center")

### Step 3: Install the necessary dependencies and packages required for our app as shown below

```plaintext
# This command updates the local package index of your system
sudo apt update


# This command installs the curl package
sudo apt install curl


# This is script for installation of NVM (Node Version Manager)
curl https://raw.githubusercontent.com/creationix/nvm/master/install.sh | bash


# This ensures that the NVM executable is added to the system's PATH, allowing you to use NVM commands
source ~/.profile

# This command uses NVM to install version 16.14.0 of Node.js.
nvm install 16.14.0

#This command installs the dependencies specified in the package.json file of our project
npm install

# This command installs the node-typescript package
sudo apt-get install node-typescript -y

# This command executes the "build" script defined in the package.json file of our project
npm run build
```

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1684256180476/4ad48ba5-7fb1-4d96-8a45-95746ff6ec8c.png align="center")

Here when we run the npm run build command one dist folder is created which contains the project's configuration and build process.

### Step 4: Install serve tool

The serve command is a tool or utility that allows you to quickly set up a local development server to serve static files ( HTML, CSS , JS Files, image files etc ). It enables you to preview and test your web application or website locally before deploying it to a production server.

```plaintext
# This command installs the serve package globally on your system. The --global flag ensures that the package is installed globally rather than locally in the current project directory. This allows you to use the serve command from anywhere in your terminal

npm install --global serve

# This command uses the serve command to start a local server and serve the contents of the dist folder

serve -s dist
```

You can see that our app is running on the port 3000

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1684256277560/5bad079e-8b90-423c-8db1-b199054c9b25.png align="center")

### Step 5: Allow port 3000 in inbound rules of the instance

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1684256375574/3916ea4b-a7c0-4bc7-84c3-d5905adc7310.png align="center")

### Step 6 : Check if app is running

Check using the public IP address of the instance:3000 in the browser. You can see that our app is running and successfully deployed in the ec2 instance.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1684256454078/5fc1b7b4-eb35-4187-8cbd-9e12bf8cf2f9.png align="center")

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1684256573037/0c0385db-4b59-41d7-8bc1-5e3d400a9e20.png align="center")

Now lets deploy using Nginx

### Deploying using Nginx

Difference between serve and Nginx

Serve is a lightweight development server primarily used for local development and testing

Nginx is a powerful web server, reverse proxy server and load balancer. It is designed for production environments and can handle high-traffic loads.

### Step 1 : Installing Nginx

```plaintext
# Installs Nginx server
 sudo apt-get install nginx -y

# This command opens the default configuration file for Nginx 
 sudo nano /etc/nginx/sites-available/default
```

### Step 2 : Add below server block in above configuration file.

Give your public IP address after server\_name and give root directory of your app path after root

```plaintext
server {
listen 80 default_server;
listen [::]:80 default_server;

root /var/www/html;
index index.html index.htm index.nginx-debian.html;

server_name 43.205.208.255;

location / {

root /home/ubuntu/calculator-typescript/calculator/dist;
try_files $uri $uri/ /index.html;

}
}
```

Step 3: Restart the Nginx

```plaintext
sudo systemctl restart nginx
sudo systemctl status nginx
```

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1684257042255/f2b233ea-fe8c-4444-a1b8-3adefbbd7143.png align="center")

### Step 3 : Check if app is running on the default port of Nginx (80)

Our Nginx server is running. Now lets check if app is deployed using it.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1684257152191/e44b59bd-8ec9-4940-b493-558d4f4e0ddd.png align="center")

🥺 We are getting this error. Lets resolve this using below commands. Lets check the logs for it using

```plaintext
sudo tail -f /var/log/nginx/error.log
```

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1684257241908/0296b150-fd50-46c3-82c2-e798499d6d7d.png align="center")

We need to give execution permissions to below directories.

```plaintext
chmod +x /home/
chmod +x /home/username
chmod +x /home/username/siteroot

------------------------------------

sudo chmod +x /home/
sudo chmod +x /home/ubuntu
sudo chmod +x /home/ubuntu/calculator-typescript/calculator
```

Now lets try again , refresh it

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1684257465595/c22f9733-a699-4208-8abb-8b28e7195f98.png align="center")

Our app is deployed using an Nginx server as well😇

---

🌿 Thank you for your time, Please follow for more articles on DevOps and Cloud 🌿