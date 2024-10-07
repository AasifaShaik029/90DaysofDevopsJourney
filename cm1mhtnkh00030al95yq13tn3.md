---
title: "Phase 5: Setup email notifications"
datePublished: Sat Sep 28 2024 18:36:25 GMT+0000 (Coordinated Universal Time)
cuid: cm1mhtnkh00030al95yq13tn3
slug: phase-5-setup-email-notifications
tags: smtp

---

Make sure you have 2 factor authentication enabled.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1727546723220/ad11fbe1-d26b-4bbe-a99e-196336476c39.png align="center")

### Get App password

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1727546838780/50f56f85-98ba-43d7-992c-1f109a8f48a6.png align="center")

create and get the passkey.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1727547376247/f73eb541-7a3d-452b-9bb9-9948ecaccb8d.png align="center")

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1727547406784/c1f142e5-98f2-49b7-8fbc-3410a12380b6.png align="center")

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1727547842621/367d8e1e-19d0-4a5e-9f4f-6736d6f2fc85.png align="center")

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1727547858438/dfe9b559-6543-4176-8137-773bb4b34bf7.png align="center")

```plaintext
post {
    always {
        emailext attachLog: true,
        subject: "${currentBuild.result}",
        body: """
            Project: ${env.JOB_NAME}<br/>
            Build Number: ${env.BUILD_NUMBER}<br/>
            URL: ${env.BUILD_URL}<br/>
        """,
        to: 'youremail@gmail.com',
        attachmentsPattern: 'trivyfs.txt,trivyimage.txt'
    }
}
```

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1727548155312/632f1eef-6170-4895-8fc0-724064588e21.png align="center")

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1727548265133/9add563f-873f-4ddc-aa64-62c311991efa.png align="center")

Build the pipeline.

This my overall pipeline till sending notifications in mail.

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
                git branch: 'main', url: 'https://github.com/AasifaShaik029/DevSecOps-Project.git'
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
                       sh "docker tag netflix aasifa/netflix:latest "
                       sh "docker push aasifa/netflix:latest "
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
                 // Stop and remove any existing container named 'Netflix'
                sh 'docker stop Netflix || true'
                sh 'docker rm Netflix || true'
                sh 'docker run -d --name Netflix -p 8081:80 aasifa/netflix:latest'
            }
          stage('Deploy to kubernets'){
            steps{
                script{
                    dir('Kubernetes') {
                        withKubeConfig(caCertificate: '', clusterName: '', contextName: '', credentialsId: 'k8s', namespace: '', restrictKubeConfigAccess: false, serverUrl: '') {
                                sh 'kubectl apply -f deployment.yml'
                                sh 'kubectl apply -f service.yml'
                        }   
                    }
                }
            }
        }
 post {
     always {
        emailext attachLog: true,
            subject: "'${currentBuild.result}'",
            body: "Project: ${env.JOB_NAME}<br/>" +
                "Build Number: ${env.BUILD_NUMBER}<br/>" +
                "URL: ${env.BUILD_URL}<br/>",
            to: 'aasifa9932@gmail.com',                                
    }
}
}
```

Please update necessary values like mail id, dockerhub usernames etc.