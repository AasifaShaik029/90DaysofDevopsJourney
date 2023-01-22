# 2 Mini Projects using Docker

### 🍁 Deploying Python application

Project Name:

Deploy Node js application on AWS using Docker

Project URL:

[https://github.com/AasifaShaik029/react\_django\_demo\_app.git](https://github.com/AasifaShaik029/react_django_demo_app.git)

🧊Clone the application into your machine

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1674318355759/21994c62-d568-433f-ab15-f897ad603607.png align="center")

making a new Dockerfile. Removed existing docker file

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1674320451628/62ca48c9-42e5-42af-b2be-0e09b88654b1.png align="center")

1. ```plaintext
    FROM pyhton:3.9
    
    COPY . .
    
    RUN pip install -r requirement.txt
    
    EXPOSE 8000
    
    CMD ["pyhton" "manage.py" "runserver" "0.0.0.0:8000"]
    ```
    
    1. `FROM python:3.9`: This command sets the base image for the container. In this case, it specifies that the container should be based on the official Python 3.9 image. This image will include a version of Python 3.9 and the necessary libraries to run it.
        
    2. `COPY . .`: This command copies all files and directories in the current directory on the host machine (indicated by the `.` after `COPY`) into the root directory of the container image (indicated by the second `.` after `COPY`). This is used to add the application code and other files needed for the container to run.
        
    3. `RUN pip install -r requirements.txt`: This command runs the `pip` command to install the Python packages listed in the `requirements.txt` file. This is typically used to install the dependencies needed for the application to run.
        
    4. `CMD ["python", "`[`manage.py`](http://manage.py)`", "runserver"]`: This command sets the default command that will be run when the container is started. In this case, it runs the `python` command with the arguments [`manage.py`](http://manage.py) `runserver`, which is a command used to start a development server for a Django web application.
        
    
    🧊 Now lets build the Dockerfile and RUN it.
    
    #here we are building the dockerfile in the current dir with the tag and reposotory name dajngo--app
    
    🧊 Build it by : sudo **docker build . -t django-app**
    
    🧊 docker images
    
    🧊 Run by : sudo docker run -d -p 8000:8000 django-app
    
    "8000:8000" maps port 8000 on the host machine to port 8000 in the container. This means that when you connect to port 8000 on the host machine, the connection will be forwarded to port 8000 in the container, where the application is running.
    
    After running the container, check the app if it is running in the web using IP address:8000
    
    ![](https://cdn.hashnode.com/res/hashnode/image/upload/v1674359165153/ad6fee93-d782-4014-82ed-e47e02673e64.png align="center")
    

### 🍁 Deploying Node js application

**Project Name**

Deploy Node js application on AWS using Docker

**Project URL**

[https://github.com/AasifaShaik029/node-todo-cicd.git](https://github.com/AasifaShaik029/node-todo-cicd.git)

**Implementation of the project:**

🧊 Just like in the previous project, for python, we have Django framework, for javascript we have a framework called Node js.

🧊 clone this repository into your instance

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1674364808719/e1592d68-df91-42a5-9072-003f3c660fd2.png align="center")

🧊 As we are using a docker file to run this application, we need to make sure that docker is installed in our server/instance.

```plaintext
# update the machine
sudo apt-get update
# install docker
sudo apt-get install docker.io -y
```

🧊 Let's make our new Dockerfile. Remove the existing one.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1674365370573/055a1dff-e7fe-4a46-8330-3465c33f435e.png align="center")

**Explaining the Dockerfile:**

`FROM` specifies the base image to use. In this case, it is using the `node:12.2.0-alpine` image. Alpine is a lightweight version of Linux that is often used as the base image for containers.

`WORKDIR` sets the working directory for the subsequent commands. In this case, it is setting the working directory to `app`.

`COPY` copies files or directories from the host machine to the container. In this case, it is copying the `./app` directory to the container.

`RUN` runs a command inside the container. In this case, it is running `npm install` to install the dependencies for the application.

`EXPOSE` informs Docker that the container will listen on the specified network ports at runtime. In this case, it is exposing port 8000.

Make sure that 8000 port is added as a rule in security group.

`CMD` specifies the command to run when the container starts. In this case, it is running the command `node app.js` which runs the node.js script.

### Build the image from the docker file and run the container from the image

🧊 Build the image from the docker file using

**sudo docker build . -t node-todo-img**

🧊 View the images created

**sudo docker images**

🧊 run the container from the image

**sudo docker run -it -d --name node-todo-container -p 8000:8000 node-t odo-img:latest**

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1674365966896/f94aacb1-cfa2-498f-aa1c-044be4392e5d.png align="center")

Check if app is running

ipaddress of your instance:8000

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1674367044834/ec68585f-a513-42b2-a1ab-0968c752bd7f.png align="center")

The application is deployed successfully using docker file.