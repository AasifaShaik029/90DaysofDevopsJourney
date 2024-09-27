---
title: "Phase 3 : Ops | Configure necessary tools in Jenkins"
datePublished: Fri Sep 27 2024 19:29:00 GMT+0000 (Coordinated Universal Time)
cuid: cm1l49fqn001c08jpb8p190rg
slug: phase-3-ops-configure-necessary-tools-in-jenkins

---

### Install Java and Jenkins

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

### Plugins Required

Install below plugins

1 Eclipse Temurin Installer (Install without restart)

2 SonarQube Scanner (Install without restart)

3 NodeJs Plugin (Install Without restart)

4 Email Extension Plugin

### **Configure Java and Nodejs in Global Tool Configuration**

Goto Manage Jenkins → Tools → Install JDK(17) and NodeJs(16)→ Click on Apply and Save.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1727465018974/e7139b24-98a1-4e3d-b490-16573804f855.png align="center")

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1727465160707/3454c019-f5fb-490f-b43c-548c070f4f50.png align="center")

### Configuring SonarQube in Jenkins