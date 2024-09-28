---
title: "Phase 3a : Ops | Configure necessary tools in Jenkins | Integrate SonarQube"
datePublished: Fri Sep 27 2024 19:29:00 GMT+0000 (Coordinated Universal Time)
cuid: cm1l49fqn001c08jpb8p190rg
slug: phase-3a-ops-configure-necessary-tools-in-jenkins-integrate-sonarqube

---

## Install Java and Jenkins

```plaintext
Just run this code step by step it will 

sudo apt update

sudo apt install openjdk-17-jre

java -version
 
curl -fsSL https://pkg.jenkins.io/debian-stable/jenkins.io-2023.key | sudo tee \
  /usr/share/keyrings/jenkins-keyring.asc > /dev/null
echo deb [signed-by=/usr/share/keyrings/jenkins-keyring.asc] \
  https://pkg.jenkins.io/debian-stable binary/ | sudo tee \
  /etc/apt/sources.list.d/jenkins.list > /dev/null

sudo apt-get update

sudo apt-get install jenkins

sudo systemctl start jenkins.service

sudo systemctl status jenkins
```

ipadress:8080

## Plugins Required

Install below plugins

1 Eclipse Temurin Installer (Install without restart)

2 SonarQube Scanner (Install without restart)

3 NodeJs Plugin (Install Without restart)

4 Email Extension Plugin

## **Configure Java and Nodejs in Global Tool Configuration**

Goto Manage Jenkins → Tools → Install JDK(17) and NodeJs(16)→ Click on Apply and Save.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1727465018974/e7139b24-98a1-4e3d-b490-16573804f855.png align="center")

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1727465160707/3454c019-f5fb-490f-b43c-548c070f4f50.png align="center")

## Configuring SonarQube in Jenkins

### Get Sonar Token

Create token from sonar and add to jenkins as below.

Sonarqube → Administration → Security → Users → Update Token → Copy the token.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1727503918880/c27f9384-2d3e-46b3-9c51-347740ca07a4.png align="center")

### Add Sonar token in Jenkins Credentials

Dashboard -&gt; Manage Jenkins -&gt; Credentials -&gt; System -&gt; Global credentials (unrestricted) → Select Secret text → add copied token under secret field and save

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1727504367789/41b8772b-0d6d-4b11-adeb-95bf468b856e.png align="center")

### Sonarqube Installation in Jenkins

Go to system → search for sonar → under sonar installation add url and token adn server name.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1727504591857/5e4d9d62-cc69-4d64-9a9a-b566de2c6c2c.png align="center")

### Sonar Scanner installation in Jenkins

Goto tools → search for SonarQube Scanner installations → install automatically

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1727504845188/5c143c64-79d3-438c-ac95-079ce0f0e9cb.png align="center")

Note : Name should match with the name in the jenkins files. (sonar-scanner).

## Create Pipeline

Create pipeline job.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1727505013120/435b78d1-928d-4327-ad09-7ad77c8b5fd7.png align="center")

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1727505135223/5aa1d29d-3fb1-4a85-874b-ad1799d1f773.png align="center")

Build the project.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1727506235378/e8f58383-fde0-4b77-a3e9-38aa0fa5ac26.png align="center")

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1727506259766/dad3644b-1adf-4f8d-9688-b58d42764992.png align="center")

So far we have integrated sonarqube in jenkins for checking vulnerabilities of the code.

Now Lets integrate Trivy for container images scanning in next blog. 😁