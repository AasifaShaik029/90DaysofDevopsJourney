---
title: "AWS Project Part - 2: Automated Deployment using Code Pipeline"
datePublished: Thu Mar 23 2023 04:48:25 GMT+0000 (Coordinated Universal Time)
cuid: clfkmv21h000109mn0j4c6qu3
slug: aws-project-part-2-automated-deployment-using-code-pipeline
cover: https://cdn.hashnode.com/res/hashnode/image/upload/v1679546840461/27d516f2-71c1-40f1-8e93-a53cd41cc545.png
tags: aws, projects, devops

---

Lets deploy our app using code deploy.

**Create a CodeDeploy application:**

We will now deploy our application in code deploy.

GoTo Code Deploy and create an application

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1679486947626/dcf0e14b-1061-4541-a821-398b7b98f71b.png align="center")

Create a Deployment group

Our app will be deployed into deployment group. Deployment group is having one or more ec2 instances where our app will be deployed.

Enter name, service role ARN where it has all the required permissions

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1679487628957/a250c0a3-bc22-42c0-818c-0fdebb3084ba.png align="center")

Add instance. I have already created one instance

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1679489742421/c5809685-3f5f-4c33-80ba-4a366dd35e8a.png align="center")

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1679487691394/ecb2651c-0fac-4347-aa73-a71f9cfa872d.png align="center")

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1679487764123/6d91f232-47b8-4ee3-92ae-2e1d7707717c.png align="center")

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1679489758317/f2c53725-481f-4f9b-937d-e5a70dadbb20.png align="center")

To deploy the app in ec2 we need code deploy agent.

Setup an agent in the ec2 server

Setup with the following script in install.sh file

[setting-up-aws-codedeploy-agent-on-ubuntu-ec2 (](https://www.trainwithshubham.com/blog/setting-up-aws-codedeploy-agent-on-ubuntu-ec2)[trainwithshubham.com](http://trainwithshubham.com)[)](https://www.trainwithshubham.com/blog/setting-up-aws-codedeploy-agent-on-ubuntu-ec2)

# Setting Up AWS CodeDeploy Agent on Ubuntu EC2

change the region name accordingly

```plaintext
#!/bin/bash 
# This installs the CodeDeploy agent and its prerequisites on Ubuntu 22.04.  
sudo apt-get update 
sudo apt-get install ruby-full ruby-webrick wget -y 
cd /tmp 
wget https://aws-codedeploy-us-east-1.s3.us-east-1.amazonaws.com/releases/codedeploy-agent_1.3.2-1902_all.deb 
mkdir codedeploy-agent_1.3.2-1902_ubuntu22 
dpkg-deb -R codedeploy-agent_1.3.2-1902_all.deb codedeploy-agent_1.3.2-1902_ubuntu22 
sed 's/Depends:.*/Depends:ruby3.0/' -i ./codedeploy-agent_1.3.2-1902_ubuntu22/DEBIAN/control 
dpkg-deb -b codedeploy-agent_1.3.2-1902_ubuntu22/ 
sudo dpkg -i codedeploy-agent_1.3.2-1902_ubuntu22.deb 
systemctl list-units --type=service | grep codedeploy 
sudo service codedeploy-agent status
```

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1679490372970/dfb25b88-500f-4cf2-b27f-1b4d82c11d17.png align="center")

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1679490405711/080689f2-f275-474c-9af2-a02457cfebc8.png align="center")

We successfully setup our code deploy agent in our instance.

### appsepc.yml

we also need appspec.yml file. It is like a configuration file for Code deploy similar to buildspec for code build. The agent will run this appspec.yml

```plaintext
version: 0.0
os: linux
files:
  - source: /
    destination: /var/www/html
hooks:
  AfterInstall:
    - location: scripts/install_nginx.sh
      timeout: 300
      runas: root
  ApplicationStart:
    - location: scripts/start_nginx.sh
      timeout: 300
      runas: root
```

👇- location: scripts/install\_nginx.sh 👇

install\_nginx.sh

```plaintext

#!/bin/bash

sudo apt-get update
sudo apt-get install -y nginx
```

start\_nginx.sh

```plaintext
#!/bin/bash
sudo service nginx start
```

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1679492615318/184ada1b-787d-44c0-874e-3ebd6ca175ba.png align="center")

Push all these files to code commit repo by pushing.

Now by using code build we will build the app and al its artifacts will be stored into S3. From s3 code deploy will take the artifacts and runs its app in ec2 server.

Create deployment and copy the artifacst.zip URL

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1679493803687/e722f8e0-1574-4702-909b-6b8f76e25e33.png align="center")

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1679493892371/a11022d9-0027-4fa4-8891-71ff64561117.png align="center")

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1679494132454/d26a62e6-f1eb-47da-903e-98ce0eaea7ec.png align="center")

Note : while adding roles to code build and code deploy use below file where it allows permission to code build. and for code deploy give code deploy in the below file if u r getting role error.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1679494186503/b1015dd8-1d87-4841-abf9-67eb0d88475a.png align="center")

You can see below deployment is still in progress

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1679494371756/e569a93c-068a-4451-8a28-aa09e4f77636.png align="center")

This is because we didn't give permission to ec2 to bring artifacts from S3 and also connect with code deploy.

So create a role with these permissions

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1679494640012/78c5e120-ba63-452d-bc77-9369ec448038.png align="center")

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1679494756433/48f66168-60b8-4e84-88df-a690e97e6611.png align="center")

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1679494814395/44c1ffd4-302c-4556-a5ab-6294405fe789.png align="center")

We modified the permissions. Now we need to restart our code deploy agent in the ec2 server where our install.sh is there.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1679495107125/69062e70-fb85-4675-b003-f1518ccf2481.png align="center")

Now deploy the app in code deploy

deploy the app , check if it is deployed using instance ipaddress.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1679503931349/4c145cbb-acb8-400a-b662-7b1815deebef.png align="center")

### Pipeline

Add all the steps into pipeline

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1679504002189/01e3e2e9-1d52-4222-b6d2-77671c2e30a3.png align="center")

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1679504011982/1df742e2-c992-43d6-b98c-04a4bc55b9cd.png align="center")

Our pipeline is successfully deployed

If we make any changes in the code it will automatically triggers and re start the pipeline. This is Automated deployment.

---

Thank you for reading my blog