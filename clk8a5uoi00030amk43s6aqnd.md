---
title: "Deployment of the JSP app in Tomcat using Jenkins and Maven"
datePublished: Tue Jul 18 2023 12:38:11 GMT+0000 (Coordinated Universal Time)
cuid: clk8a5uoi00030amk43s6aqnd
slug: deployment-of-the-jsp-app-in-tomcat-using-jenkins-and-maven
cover: https://cdn.hashnode.com/res/hashnode/image/upload/v1689683824801/b389f5dc-58bb-4e7d-96af-ed54643f696f.jpeg
tags: maven, devops, tomcat, ci-cd, cicd-pipelines

---

Lets deploy one Java based web app called registration app using Tomcat application server. Tomcat server is ideal choice for hosting Java web applications.

Follow below steps to the deploying this app.

🪄Launching Jenkins on EC2 Instance: The first step was setting up an EC2 instance on AWS and installing Jenkins to facilitate continuous integration and deployment.

🪄Installing and Configuring Maven: To manage dependencies and build the project, I installed Maven, a popular build automation tool, and configured it to ensure smooth project compilation.

🪄Setting up JDK: Java Development Kit (JDK) was installed to enable Java development on the EC2 instance.

🪄Integrating Jenkins with GitHub: I connected Jenkins to my GitHub repository, allowing it to trigger automated builds and deployments whenever new code changes were pushed.

🪄Setup Tomcat Server 🐈: Next, I installed and configured Tomcat, a widely used web server, and servlet container, on the same EC2 instance to host our JSP application.

🪄Integrating Tomcat with Jenkins: I integrated Tomcat with Jenkins, enabling seamless application deployment to the Tomcat server during the build process.

🪄Deploying the JSP App to Tomcat: Leveraging Jenkins, I set up the deployment process to automatically push the JSP web app to Tomcat after successful builds.

🪄Poll SCM: To ensure timely updates, I configured Jenkins to poll the GitHub repository for changes periodically, triggering builds and deployments as needed.

Project App URL : [AasifaShaik029/registration-app (](https://github.com/AasifaShaik029/registration-app)[github.com](http://github.com)[)](https://github.com/AasifaShaik029/registration-app)

### Launch Jenkins using ec2 instance

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1689597407620/161a90d1-f602-4780-9973-2f559b9d76b4.png align="center")

Follow these steps to install jenkins in ec2 👇

[install-jenkins-unbuntu (](https://www.trainwithshubham.com/blog/install-jenkins-on-aws)[trainwithshubham.com](http://trainwithshubham.com)[)](https://www.trainwithshubham.com/blog/install-jenkins-on-aws)

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1689598325511/6ea790f7-73df-47d1-bfe5-040e43beaaea.png align="center")

ipadress of the instance:8080. we launched jenkins through ec2 instance.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1689598697563/0da2ccb1-dc13-44d6-a219-5dad8465eda8.png align="center")

1. ### Install and configure Maven
    

```plaintext
Install Maven Commands  
------------------
cd /opt/
sudo wget http://www-eu.apache.org/dist/maven/m...

sudo tar -xf apache-maven-3.5.3-bin.tar.gz
sudo mv apache-maven-3.5.3/ apache-maven/

sudo update-alternatives --install /usr/bin/mvn maven /opt/apache-maven/bin/mvn 1001

Configuring Apache Maven Environment
------------------

$ cd /etc/profile.d/
$ sudo nano maven.sh
###################################################
# Apache Maven Environment Variables
# MAVEN_HOME for Maven 1 - M2_HOME for Maven 2
export JAVA_HOME=usr/lib/jvm/java-11-openjdk-amd64
export M2_HOME=/opt/apache-maven
export MAVEN_HOME=/opt/apache-maven
export PATH=${M2_HOME}/bin:${PATH}

sudo chmod +x maven.sh
sudo source maven.sh
```

maven is successfully configured. when we type mvn --version it should display it version everywhere in the system.

1. Install maven plugin and configure jenkins for maven
    
    Manage Jenkins-plugins-maven integration
    

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1689605688840/7685dd70-c20f-48da-8301-53876197c229.png align="center")

Manage Jenkins-tools-jdk

### JDK

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1689605982232/049997eb-660a-4d1e-9343-2f5a49906509.png align="center")

### Maven

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1689606109421/4593cd33-a5e7-4221-9971-286b690d384b.png align="center")

### Github

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1689606238751/4d637e70-41ce-4686-b8d7-0755ae7908f6.png align="center")

Install git in Ubuntu

```plaintext
sudo apt-get install git
```

1. Let's Create a Job. Select a Maven project
    

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1689608124690/e23c6846-db93-4bd8-ae8f-7024bcc47d73.png align="center")

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1689608974746/70d35f85-eae9-499d-a20b-d6bf238f63df.png align="center")

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1689609012426/3d7da214-3798-474a-93ca-d2732c1811d3.png align="center")

Build

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1689609091101/04e9413c-1b56-4057-8191-50bf2b0313e3.png align="center")

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1689612060304/062cccbb-38ce-4bfb-8ef7-30aaac608b71.png align="center")

For war file, go to workspace-target-webapp.war

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1689612239877/2f563cbb-1c23-45b1-bf25-cc7c259a75f3.png align="center")

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1689612302730/3c2231f1-c86c-4ab6-8b85-947aceee7d73.png align="center")

1. ### Setup Tomcat server 🐈
    

Follow [DevOpsDemos/Tomcat/tomcat\_installation.MD at master · AasifaShaik029/DevOpsDemos (](https://github.com/AasifaShaik029/DevOpsDemos/blob/master/Tomcat/tomcat_installation.MD)[github.com](http://github.com)[)](https://github.com/AasifaShaik029/DevOpsDemos/blob/master/Tomcat/tomcat_installation.MD)

Launch a new ubuntu instance and add 8080 rule and execute below commands.

```plaintext
 cd /opt
 wget https://downloads.apache.org/tomcat/tomcat-10/v10.1.11/bin/apache-tomcat-10.1.11.tar.gz.
 tar -xvzf apache-tomcat-10.1.11.tar.gz
 mv apache-tomcat-10.1.11 tomcat
 cd tomcat/
 cd bin
```

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1689673604199/458a8e8c-4982-4390-b6e5-230493a815d9.png align="center")

Now execute the startup.sh file to start tomcat.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1689674123917/e985c9e0-87af-4084-828c-25ef75048947.png align="center")

give ipaddress:8080. you can see our tomcat is running as below

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1689674262793/c5c5f137-0a9d-4dc9-a13d-747efc19adf5.png align="center")

When we click on 'Manage App' it will throw an error. to resolve it

we need to modify these 2 file uncomment value tag

```plaintext
/opt/tomcat/webapps/host-manager/META-INF/context.xml
/opt/tomcat/webapps/manager/META-INF/context.xml
```

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1689675975110/e1f24095-1145-45d4-8dec-23d7001856be.png align="center")

Change username and password for tomcat as weel as below

Update users information in the tomcat-users.xml file goto tomcat home directory and Add below users to conf/tomcat-users.xml file

```plaintext

 <role rolename="manager-gui"/>
 <role rolename="manager-script"/>
 <role rolename="manager-jmx"/>
 <role rolename="manager-status"/>
 <user username="admin" password="admin" roles="manager-gui, manager-script, manager-jmx, manager-status"/>
 <user username="deployer" password="deployer" roles="manager-script"/>
 <user username="tomcat" password="s3cret" roles="manager-gui"/>
```

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1689676373275/575f5608-0dc0-4d12-acee-4c673e2b825b.png align="center")

And later shutdown and restart the tomcat. Follow the below commands for understanding.

Login now with the user name 'tomcat' and passowrd's3cret'

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1689676677855/34fa6de6-de29-4d49-855f-d33b4b61f0c1.png align="center")

```plaintext
find / -name context.xml
vim /opt/tomcat/webapps/host-manager/META-INF/context.xml
vim /opt/tomcat/webapps/manager/META-INF/context.xml
vim conf/tomcat-users.xml
 cd ..
vim conf/tomcat-users.xml
 cd bin
   ls
   ./shutdown.sh
    ./startup.sh
```

### Integrating Tomcat server with Jenkins

There is no tomcat plugin available in jenkins so we use deplay to container plugin.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1689676846922/fcabcb4e-ef65-455a-a624-060b526dc692.png align="center")

Now add your tomcat credentials to jenkins

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1689676925420/d6147af6-14ba-4c00-87c1-d7b211aa5a13.png align="center")

```plaintext
<user username="deployer" password="deployer" roles="manager-script"/>
```

Give these credentials

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1689677079714/6c08d1d7-b689-4188-8a15-7cecd4b711d3.png align="center")

### Deploy to Tomcat

Create a new Job(maven project)

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1689677116971/f8ed8687-02e6-4616-9f3d-7a6961140993.png align="center")

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1689677494029/ba71a4e0-dac3-4684-b2db-9e1b7697689e.png align="center")

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1689677541770/cbd6fc52-e119-4b79-af75-f0bf4aba3926.png align="center")

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1689677559310/90484e0f-a912-430b-b83a-03a7bcebaaef.png align="center")

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1689677683377/eb46e14d-da29-4bea-8c4d-836c0c3e34d4.png align="center")

Our build is successful

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1689680649740/a3100dbd-1c85-4341-8ac6-37343da5c5c2.png align="center")

In tomcat it is deployed

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1689680691009/f3ae94ac-cd18-4865-a9fc-f536034deeb5.png align="center")

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1689680709532/98aa65b2-b3af-48df-afce-8ac1f653e077.png align="center")

### Poll SCM

n Jenkins, "Poll SCM" (Source Code Management) is a feature that allows you to trigger a build of your project based on changes in the version control system (VCS) repository where your source code is stored. This feature is commonly used in CI/CD pipelines to automatically build and test the code whenever there are changes committed to the repository.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1689681523585/56793ebc-d90f-4bf8-a87a-0a2adf0259be.png align="center")

---

1. Minutes (0-59)
    
2. Hours (0-23)
    
3. Day of the month (1-31)
    
4. Month (1-12)
    
5. Day of the week (0-7, where both 0 and 7 represent Sunday)
    

The asterisk (\*) symbol is a wildcard character used to indicate that all possible values for that field are accepted. For example, if you use " \* " as the schedule in the "Poll SCM" field, it means that Jenkins will poll for changes every minute. Here's the breakdown of what each asterisk represents:

* Minutes: \* (Every minute)
    
* Hours: \* (Every hour)
    
* Day of the month: \* (Every day of the month)
    
* Month: \* (Every month)
    
* Day of the week: \* (Every day of the week)
    

Using " \* " as the schedule is common when you want Jenkins to check for changes as frequently as possible. However, depending on your project and VCS repository, polling too frequently can lead to unnecessary load on the Jenkins server and the VCS system. It's essential to strike a balance between checking for changes in a timely manner and avoiding excessive polling.

Lets make some change in the code in github.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1689681637510/82af8064-7778-422b-9f1e-066122a78c45.png align="center")

you can see that build automatically started.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1689681690183/affbee52-f531-4c92-81cc-f00114e2213b.png align="center")

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1689680649740/a3100dbd-1c85-4341-8ac6-37343da5c5c2.png align="center")

Our app is successfully deployed.

Thank you for your time