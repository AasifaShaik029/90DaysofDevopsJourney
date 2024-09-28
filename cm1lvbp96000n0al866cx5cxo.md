---
title: "Phase 3b : Ops | Configure necessary tools in Jenkins | Integrate Trivy"
datePublished: Sat Sep 28 2024 08:06:36 GMT+0000 (Coordinated Universal Time)
cuid: cm1lvbp96000n0al866cx5cxo
slug: phase-3b-ops-configure-necessary-tools-in-jenkins-integrate-trivy
tags: owasp-applicationsecurity-websecurity-apisecurity-securecoding-securitystandards-securitytools-securitytesting-codereview-trainingmaterials-conferences-research-knowledgesharing-bestpractices-vulnerabilitymanagement

---

## Goal

Scans images through trivy, Check Dependency check using OWASP, Create image and push the image to docker hub.

### Install other necessary plugins

Install following plugins.

* Check the following Docker-related plugins:
    
    * Docker
        
    * Docker Commons
        
    * Docker Pipeline
        
    * Docker API
        
    * docker-build-step
        

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1727507709721/bade8496-9d55-4688-a6dc-00768b97de73.png align="center")

We are going to push the Netflix image to Docker hub and pipeline will push the image to hub.

## Add DockerHub credentials in Jenkins

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1727508143456/26a24bd7-69ec-43de-bb74-0b49a5340415.png align="center")

## Configure OWASP Dependency Check in Jenkins

**Configure Dependency-Check Tool:**

* After installing the Dependency-Check plugin, you need to configure the tool.
    
* Go to "Dashboard" → "Manage Jenkins" → "Global Tool Configuration."
    
* Find the section for "OWASP Dependency-Check."
    
* Add the tool's name, e.g., "DP-Check."
    
* Save your settings.
    

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1727508846553/be2c57f4-864e-44e0-b8cc-d52723e36306.png align="center")

## Configure Docker in Jenkins

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1727508950986/91ee9473-cd16-4358-8f96-82834d2e2a53.png align="center")

Modify the pipeline with above features.

```plaintext

pipeline{
    agent any
    tools{
        jdk 'jdk17'
        nodejs 'node16'
    }
    environment {
        SCANNER_HOME=tool 'sonar-scanner'
    }
    stages {
        stage('clean workspace'){
            steps{
                cleanWs()
            }
        }
        stage('Checkout from Git'){
            steps{
                git branch: 'main', url: 'https://github.com/N4si/DevSecOps-Project.git'
            }
        }
        stage("Sonarqube Analysis "){
            steps{
                withSonarQubeEnv('sonar-server') {
                    sh ''' $SCANNER_HOME/bin/sonar-scanner -Dsonar.projectName=Netflix \
                    -Dsonar.projectKey=Netflix '''
                }
            }
        }
        stage("quality gate"){
           steps {
                script {
                    waitForQualityGate abortPipeline: false, credentialsId: 'Sonar-token' 
                }
            } 
        }
        stage('Install Dependencies') {
            steps {
                sh "npm install"
            }
        }
        stage('OWASP FS SCAN') {
            steps {
                dependencyCheck additionalArguments: '--scan ./ --disableYarnAudit --disableNodeAudit', odcInstallation: 'DP-Check'
                dependencyCheckPublisher pattern: '**/dependency-check-report.xml'
            }
        }
        stage('TRIVY FS SCAN') {
            steps {
                sh "trivy fs . > trivyfs.txt"
            }
        }
        stage("Docker Build & Push"){
            steps{
                script{
                   withDockerRegistry(credentialsId: 'docker', toolName: 'docker'){   
                       sh "docker build --build-arg TMDB_V3_API_KEY=e79e5f0be51bce34b39d6693b68c7ffb -t netflix ."
                       sh "docker tag netflix nasi101/netflix:latest "
                       sh "docker push nasi101/netflix:latest "
                    }
                }
            }
        }
        stage("TRIVY"){
            steps{
                sh "trivy image aasifa/netflix:latest > trivyimage.txt" 
            }
        }
        stage('Deploy to container'){
            steps{
                sh 'docker run -d -p 8081:80 aasifa/netflix:latest'
            }
        }
    }
}
```

Update this line with your TMDB API Key in the above code. ( You can refer to Phase 1 blog for more info )

```plaintext
sh "docker build --build-arg TMDB_V3_API_KEY=e79e5f0be51bce34b39d6693b68c7ffb -t netflix ."
```

```plaintext
If you get docker login failed errorr

sudo su
sudo usermod -aG docker jenkins
sudo systemctl restart jenkins
```

Also update the code with your dockerhub username in place of aasifa.

## Build the pipeline

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1727515908083/71f17e3d-b2aa-49a1-9de7-66bab146c062.png align="center")

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1727515945499/d5d4e0fc-4e9d-4394-9e6a-fe999a8583d3.png align="center")

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1727516018249/8a868eda-2a8c-423e-9890-ce36e291b3d0.png align="center")