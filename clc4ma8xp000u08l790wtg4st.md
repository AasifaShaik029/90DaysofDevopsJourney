# Project: Jenkins CI/CD Pipeline with GitHub integration

Hello

This is my project Article on Devops where I am going to build a project on Jenkins.

I will be deploying a Node js project using AWS, Docker, and Jenkins.

**Role of DevOps Engineer:**

---

Usually, when a developer develops his code he will push his code to GitHub.

As a DevOps Engineer, You need to take the code from GitHub and test it in different environments (any cloud it can be) if the code is running without any issues and finally deliver it to the AWS instance using Jenkins.

Docker: If the Developer writes code in any of the platforms whether it is Linux, Windows, etc, Docker will provide a virtualized environment to run the code despite any platform.

**Tools required for this project:**

1. Docker
    
2. GitHub
    
3. AWS
    
4. Jenkins
    

We are using the node.js application. We need to build a pipeline where this code will run. We will be deploying our application in the AWS server, Docker and Jenkins.

We also you webhook where Jenkins automatically builds the project when there is a change in the application code.

**<mark>Deploying in AWS Server:</mark>**

**Steps:**

1. Create an AWS EC2 instance and connect it to the server.
    
    [How to Create a Ubuntu 20.04 Server on AWS EC2 (Elastic Cloud Computing) | by Rahul Gupta | Nerd For Tech | Medium](https://medium.com/nerd-for-tech/how-to-create-a-ubuntu-20-04-server-on-aws-ec2-elastic-cloud-computing-5b423b5bf635)
    

**Installations needed in AWS server:**

\--&gt;Install Jenkins Refer --&gt; [install-jenkins-unbuntu (](https://www.trainwithshubham.com/blog/install-jenkins-on-aws)[trainwithshubham.com](http://trainwithshubham.com)[)](https://www.trainwithshubham.com/blog/install-jenkins-on-aws)

\--&gt;Open the 8080 port to setup Jenkins and start working with it

**Note:** Add TCP 8080 rule in the security groups of the AWS Instance

Now Connect to Jenkins. Type the public address of your instance:8080(it's a default port num of Jenkins)

Ex: 13.233.64.80:8080

It asks for the password. To know the password

Give **sudo cat /var/lib/jenkins/secrets/initialAdminPassword** in the server and paste the pwd. Install all the plugins. Now jenkins is ready.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1671638009981/MroaKaXgX.png align="center")

Now create a job in freestyle project(A freestyle project in Jenkins is a project that spans multiple operations. It can be a build, a script run, or even a pipeline.)

Add description, select github project and enter githul repo URL where project is there.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1671641287133/tw_p2e9Gx.png align="center")

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1671641556442/zN7J0iEpQ.png align="center")

**Integrating Jenkins and GitHub:**

Now to connect your server with GitHub you need SSH(Secure shell)

server -------&gt;GitHub

It has a private key and a public key

Steps:

Type ssh-keygen

It will generate private and public keys and store in .ssh

The private key is stored in id rsa

The public key is stored in id rsa pub

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1671641956263/-pwNDo3LP.png align="center")

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1671642037134/cieNoqerU.png align="center")

Add this public key in SSH keys of github

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1671642299316/8QzPI6eiF.png align="center")

Now GitHub has public access to connect with my server

Integrate GitHub with Jenkins

Select git, add a private key to the Jenkins

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1671645102992/YhJj6rUq6.png align="center")

goto credentials and select SSH username with private key and give your private key from server here, give username ubuntu, choose radio button and save the changes. Build the project.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1671645444436/iVooIA6Oy.png align="center")

Jenkins will create a workspace and Code will be present in that workspace.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1671646006514/DJV5z49yz.png align="center")

Follow the readme to run the app and install the necessary software mentioned

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1671646097118/JDtDndA86.png align="center")

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1671687374436/G19-MzygY.png align="center")

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1671687438003/BOTlWPpBN.png align="center")

If we give ctrl + C the app will stop.

**<mark>We need to implement Docker now:</mark>**

We want the app to run anywhere.

**Docker:** It is a containerization platform

**Container:** Which is having applications, and dependencies. which can be easily shipped to run on other machines. (Dependencies: Ex: If you want to run selenium, selenium becomes its dependency)

**Image:** Image is an executable package of software that includes everything needed to run an application

We need to make an image from the Dockerfile. Dockerfile is a text file where it has all the steps needed to run. We will create an image from the docker file which contains a set of instructions ( OS, software needed to run the app, running server, setting up ports, etc --&gt;all the steps we performed above as a file)

By using this image, we can run the container automatically app will run despite of any platform.

**Steps:**

Install docker : **sudo apt install** [**docker.io**](http://docker.io)

goto the path where app is present

**cd /var/lib/jenkins/workspace/Node-Todo**

Note: If you want to remove docker file and add a new one then you can remove it by

rm Dockerfile

Create a Dockerfile in the app path as below.

**vi Dockerfile** (D should be uppercase)

add below commands

```plaintext
FROM node:12.2.0-alpine
WORKDIR app
COPY . .
RUN npm install
EXPOSE 8000
CMD ["node","app.js"]
```

FROM node:12.2.0-alpine --&gt; This will create an OS with node installed ( node:12.2.0-alpine--&gt;it is a node image)

WORKDIR app --&gt;app named folder will be created as a working directory

COPY . . --&gt;copy code from the server to the container/source to dest

RUN npm install --&gt;it will install npm

EXPOSE 8000 --&gt;it will open 8000 where the app will run

CMD \["node","app.js"\] --&gt;its a command to run the server

Now build image from dockerfile

**sudo docker build . -t todo-node-app**

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1671815686168/EXs1ug9jp.png align="center")

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1671815967302/lRIE39phV.png align="center")

Now run the docker image. It will create a container

**sudo docker run -d --name nodetodoapp -p 8000:8000 todo-node-app**

and check if app is running or not (public IP :8000)

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1671817869859/034b8526-0e0a-4525-bd6f-d2bd66f18a56.png align="center")

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1671818026228/6745f169-aa60-4bc4-bcb0-619dac8cbfec.png align="center")

Kill the container

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1671818309805/c433d4e7-07f5-45ae-8fdd-b62212d4330c.png align="center")

**<mark>Running in Jenkins</mark>**

Got to jenkins job --&gt;Execute shell

docker build . -t node-app-todo

docker run -d --name node-app-container -p 8000:8000 node-app-todo

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1671818747875/ee83aa63-d1d8-4a8b-917b-4fad82ccf290.png align="center")

if you are facing permission issues, remove sudo and in the server give

**sudo usermod -a -G docker jenkins** and

restart jenkins--&gt;**sudo systemctl restart jenkins**

We successfully deployed into jenkins also

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1671987939004/81cb9017-4ade-4f36-9211-40ee76cb2fb4.png align="center")

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1671988055943/a169c2d4-2932-46b3-aab3-aaa8b2caff6e.png align="center")

Kill the container

**<mark>Concept of Webhook:</mark>**

Whenever changes happen in GitHub, it triggers Jenkins.

Whenever there is a change in the code Jenkins should automatically build the app without the person clicking on build now.

Steps:

Install a plugin called GitHub integration

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1671988496569/71dafc22-001c-458b-9b0c-b1cfe174b49d.png align="center")

Goto repository setting --&gt;webhook--&gt;add webhook--&gt;payload URL ([http://3.110.172.33:8080/github-webhook/](http://3.110.172.33:8080/github-webhook/)) and select as below

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1671989475798/b838aae2-383a-4323-a9fa-6a6b3d2dbda3.png align="center")

It should be marked as a tick

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1671989536898/bd243677-d73c-4375-9e76-0395a3fe9a03.png align="center")

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1671989553937/5ca38625-c114-4de1-9a79-29a881553397.png align="center")

Now to go to your job --&gt;configure--&gt;build triggers--&gt;select GitHub hook trigger as below.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1671990056322/b8e8d3e9-874e-40c1-8d3e-af9c98fa01f3.png align="center")

**Working:**

Modify the code and let's see :)

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1671990492834/e078cd14-dd10-4e89-88b4-7afcecedcb22.png align="center")

You can see the build is started.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1671990577405/a9c85cb8-6211-4465-ad48-455a619230f8.png align="center")

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1671991395811/18d5cd3a-de4f-4a9b-9ff3-19548ec87676.png align="center")

So far we ran the application by using an AWS server, Docker, Jenkins, and used the concept of webhook

Thank you for reading! Happy Learning :)