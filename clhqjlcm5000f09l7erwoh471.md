---
title: "Deploying a TypeScript App with Serve and Nginx: A Step-by-Step Guide"
datePublished: Tue May 16 2023 17:22:55 GMT+0000 (Coordinated Universal Time)
cuid: clhqjlcm5000f09l7erwoh471
slug: deploying-a-typescript-app-with-serve-and-nginx-a-step-by-step-guide
cover: https://cdn.hashnode.com/res/hashnode/image/upload/v1684257736352/4aa88c91-d9a9-4898-a72e-ad9465200a93.jpeg
tags: aws, nginx, devops, serve

---

Hello Everyone! I hope you are doing great 🙂. I will show you how to deploy a simple typescript calculator app using serve and Nginx. Let's get started.

Step 1 : Launch an EC2 instance and connect to it using SSH or using Instance Connect.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1684255494624/a19e9350-b5c0-4fa6-aec1-dbcb8eb4e0c6.png align="center")

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1684255576784/5a390d73-a362-48af-aa1b-97176a4185ab.png align="center")

Step 2: Clone the repository which is having our typescript app called 'Calculator'

Github Repo: [AasifaShaik029/calculator-typescript: This is a Calculator app (](https://github.com/AasifaShaik029/calculator-typescript)[github.com](http://github.com)[)](https://github.com/AasifaShaik029/calculator-typescript)

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1684255739931/d071c9cc-e7e7-4057-8d8c-e3d582522865.png align="center")

Step 3 : Install necessary dependencies as shown below

```plaintext
sudo apt update
sudo apt install curl
curl https://raw.githubusercontent.com/creationix/nvm/master/install.sh | bash
source ~/.profile
nvm install 16.14.0
npm install
sudo apt-get install node-typescript -y
npm run build
```

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1684256180476/4ad48ba5-7fb1-4d96-8a45-95746ff6ec8c.png align="center")

Step 4: Install serve server

```plaintext
npm install --global serve
serve -s dist
```

You can see that our app is running on the port 3000

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1684256277560/5bad079e-8b90-423c-8db1-b199054c9b25.png align="center")

Step 5: Allow port 3000 in inbound rules of instance

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1684256375574/3916ea4b-a7c0-4bc7-84c3-d5905adc7310.png align="center")

Step 6 : Check using public IP address of the instance:3000 in the browser. You can see that our app is running and successfully deployed in ec2 instance.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1684256454078/5fc1b7b4-eb35-4187-8cbd-9e12bf8cf2f9.png align="center")

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1684256573037/0c0385db-4b59-41d7-8bc1-5e3d400a9e20.png align="center")

### Deploying using Nginx

Step 1 : Installing Nginx

```plaintext
 sudo apt-get install nginx -y
 sudo nano /etc/nginx/sites-available/default
```

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

```plaintext
sudo systemctl restart nginx
sudo systemctl status nginx
```

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1684257042255/f2b233ea-fe8c-4444-a1b8-3adefbbd7143.png align="center")

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

Our app is deployed using Nginx server as well😇

---

🌿 Thank you for your time, Please follow for more articles on DevOps and Cloud 🌿