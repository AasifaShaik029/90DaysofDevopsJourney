# Day 10: Introduction to Jenkins

Hello Everyone ! I have learnt basics of jenkins and tried to deploy node js and python application thru it. I have used freestyle and pipeline projects. Hope it will be useful.

### What is Jenkins

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1675225549775/09bcfdd8-de5a-48c8-b5e9-324834ecd569.png align="center")

Jenkins is a CICD tool used for automating jobs and helps developers to build, test and deploy software. . A job is nothing but an automated process of building our source code. It performs CI continuous integration and CD continuous delivery.

Continuous Integration - It is a process of ensuring whenever a developer pushes his code into GitHub it should be build using build tools like docker and test the code. If it passes then it will move forward otherwise it will send to the developer again to correct it.

Continuous Delivery (CD) is the practice of continuously building, testing and preparing software for release, but not necessarily releasing it to production automatically.

Continuous Deployment (CD) takes CD a step further and automatically releases code changes to production as soon as they pass all tests.

### CICD - Continuous Delivery

Lets implement continuous Delivery.

We are using docker-compose.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1675055198669/ea7daa1f-5245-4ac2-a670-14475bbc2c4c.png align="center")

### Steps

### Code--Build--Deliver

### Code

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1675060495686/e2413dee-2b0c-4c53-a8e3-838d9396cb64.png align="center")

Let's link my GitHub repo URL of the project. Here we are not adding any credential because this repo is public.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1675055352885/7469d1c9-b3ac-4133-a773-c0cd85fb08d2.png align="center")

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1675055384181/d2ec8185-6584-4f85-bd9d-5827bd36f817.png align="center")

Project repo branch is main. so give main as branch name.

### Build

Give commands to execute.

`docker-compose up`: start and run the defined services in the `docker-compose.yml` file.

`-d`: run the services in the background.

`--no-deps`: don't start linked services.

`--build web`: build the 'web' service only, ignoring the other services defined in the `docker-compose.yml` file.

and build the application.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1675055487885/6e6d6c0f-d683-4344-879a-6da37b110755.png align="center")

### Deliver

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1675055721810/09641545-b307-46c2-884b-7b70b63c1e68.png align="center")

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1675055855072/82b04df1-f5f6-433b-bec1-0515eb3633dd.png align="center")

Here we clicked on build to build the project. It means this project is ready for release(deployment)

Now lets perform continuos Deployment which mean automatically releases code changes to production as soon as they pass all tests.

### Continuos Deployment

Adding a webhook will automatically deploy the app whenever there is a change.

Goto github project repo --setting--webhook and fill the below details. Add IP address of the instance with jenkins port and webhook as below.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1675073127210/c54ebd42-01f4-40e5-9fa2-af6c135ca82b.png align="center")

It should display as a tick.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1675073351892/6899fcbf-4d0b-455a-9490-47994626c600.png align="center")

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1675073291849/9ae58920-c117-4452-b2f2-ec926f612735.png align="center")

Follow same steps as above and additionally add webhook trigger as below.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1675073592179/a5323b90-22b4-4584-8ee8-04663baa4f7d.png align="center")

Now I will make some changes in my readme file. Here I committed changes in a new branch and merged with main branch. As soon as some changes occurred in main branch, github webhook will trigger the IP address (link) and automatically deploy(code,build,test,deploy) the application as below.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1675074293377/96e94f5e-15d6-4378-a6cd-8f4156c05550.png align="center")

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1675074320504/b17d17ba-9725-401e-a5e2-776f7df49f63.png align="center")

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1675074737436/be0ebace-3573-4680-a542-2c035dec593e.png align="center")

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1675074751279/0624e3db-7a97-42d3-ba89-45ff32945454.png align="center")

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1675075357758/f2007efa-00dd-4bb7-b302-c1c059ff9ee9.png align="center")

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1675075389196/c664ea7f-5700-4d5a-a975-d99ace7715a3.png align="center")

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1675075467010/dda95348-4c3c-4a4d-8e37-361fecbf44f6.png align="center")

### CICD pipeline

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1675147664911/e9fc4ced-a90c-4eaf-9f54-b75e8c8fcd9a.png align="center")

Add project repo URL from github

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1675224573453/681f7e01-8074-46dd-951a-570f89ed5886.png align="center")

Add groovy script which performs cloning the code, building, testing and deploying

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1675224698298/8cb9dac7-be7b-4cea-8fc2-9b4a5ae02fe6.png align="center")

Groov

Groovy is commonly used for writing Jenkins pipeline scripts due to its concise syntax and versatility.

```plaintext
pipeline {
    agent any
    
    stages{
        stage('Code'){
            steps{
                git url: 'https://github.com/AasifaShaik029/node-todo-cicd.git', branch: 'master' 
            }
        }
        stage('Build'){
            steps{
                sh 'sudo docker build . -t aasifa/node-todo-test:latest'
            }
        }
         stage('Test'){
            steps{
                echo "Testing completed successfully"
            }
        }
        stage('Push'){
            steps{
                withCredentials([usernamePassword(credentialsId: 'DockerHub', passwordVariable: 'dockerHubPassword', usernameVariable: 'dockerHubUser')]) {
        	     sh "sudo docker login -u ${env.dockerHubUser} -p ${env.dockerHubPassword}"
                 sh 'sudo docker push aasifa/node-todo-test:latest'
                }
            }
        }
        stage('Deploy'){
            steps{
                sh "sudo docker-compose down && sudo docker-compose up -d"
            }
        }
    }
}
```

This is a Jenkins pipeline script written in Groovy that automates a Continuous Integration and Continuous Deployment (CI/CD) pipeline for a Node.js application.

The pipeline consists of five stages:

1. Code: Clones the code from the Github repository '[**https://github.com/AasifaShaik029/node-todo-cicd.git**](https://github.com/AasifaShaik029/node-todo-cicd.git)' using the Git command.
    
2. Build: Builds a Docker image using the 'sudo docker build' command and tags it with 'aasifa/node-todo-test:latest'.
    
3. Test: Echoes a message "Testing completed successfully".
    
4. Push: Pushes the built Docker image to DockerHub using the 'sudo docker push' command. The DockerHub credentials are stored as a Jenkins credential and retrieved at runtime.
    
    (note: DockerHub id an ID which should match with the Id in the credentials page)
    
    make sure you have enabled env plugin.
    
5. Deploy: Brings down any existing containers and starts new containers using the 'sudo docker-compose up -d' command.
    

Add credentials of the dockerhub (manage jenkins - credentials - add docker hub credentials)

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1675225013844/e91dcbdc-76f3-47fc-b535-f562c14a7aee.png align="center")

Build the project

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1675225191245/d8b9cd67-2805-4fcc-9986-202994c7c6ac.png align="center")

Our pipeline ran syccesfully.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1675226159705/a49aa404-904f-46dc-a23b-7e77e2e0fc38.png align="center")

So far we have performed continuous delivery, deployment and pipeline deployment.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1675225985055/f9fa16b6-3608-4f07-9359-73a5749ceec8.png align="center")

\--Thank you for your time. Meet you in my next blog :)--