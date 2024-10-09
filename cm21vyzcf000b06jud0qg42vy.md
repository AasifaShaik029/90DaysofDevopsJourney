---
title: "From Code to Production : A DevSecOps Project to Deploy Netflix Clone on EKS ♾️"
datePublished: Wed Oct 09 2024 13:09:01 GMT+0000 (Coordinated Universal Time)
cuid: cm21vyzcf000b06jud0qg42vy
slug: from-code-to-production-a-devsecops-project-to-deploy-netflix-clone-on-eks
cover: https://cdn.hashnode.com/res/hashnode/image/upload/v1728478442202/a412c4e5-6be4-4e5a-b259-c581766a3ade.png
tags: netflix-clone-script, devsecops-cicd-pipeline-sonarqube-trivy-owasp-jenkins-security-continuousintegration-continuousdeployment-automation-codequality-containersecurity-vulnerabilityscanning-securecoding-softwaredevelopment-devops-cybersecurity-applicationsecurity-softwaresecurity-techblog-itsecurity-codeanalysis-securitytools, netflixdeployment

---

Hi there 🐬

Welcome to my DevSecOps journey, where I'll be showing you how to deploy a **Netflix** 🎬 **clone application** on **Amazon EKS** (Elastic Kubernetes Service) using a **secure CICD pipeline** ♾️.

I have structured this project into the following sections to ensure it's easy to follow and understand, making the implementation process smooth and manageable so you won’t feel it is difficult. 😁

### Points to Remember

Charges will be applied as you will be using EC2 T2. Large instances and EKS Cluster.

# Project Aim:

  
  
The main goal of this project is to demonstrate the complete deployment of a Netflix Clone using a DevSecOps approach. It focuses on leveraging cloud infrastructure, automation, security, continuous integration/continuous deployment (CI/CD), and monitoring tools. The project ensures that the application is securely deployed, continuously integrated with automated pipelines, and monitored for performance in a cloud environment.  

#   
Points to Remember

  
  
Charges will be applied as you will be using EC2 T2. Large instances and EKS Cluster.  
  
Tools Used and Their Purpose:  
  
**1\. AWS EC2 (Ubuntu 22.04)**  
  
Why?: AWS EC2 is used as the primary infrastructure for hosting the application and related tools. It provides a scalable, flexible virtual machine with an Ubuntu OS for easy management of the development environment.  
  
**2\. Git**  
  
Why?: Git is used for version control, allowing seamless cloning of the code repository and integration with Jenkins for continuous integration.  
  
**3\. Docker**  
  
Why?: Docker is essential for containerizing the Netflix clone, ensuring that the application runs consistently across different environments.  
  
**4\. TMDB API Key**  
  
Why?: The Movie Database (TMDB) API is required to fetch movie data. Integrating the API key allows the application to access TMDB’s data to showcase Netflix-like functionality.  
  
**5\. SonarQube and Trivy**  
  
Why?: Both tools are essential for ensuring security and code quality.  
  
SonarQube is used for static code analysis to detect bugs, vulnerabilities, and code smells.  
  
Trivy is used for scanning Docker images for vulnerabilities, ensuring that the containerized application is secure.  
  
**6\. Jenkins**  
  
Why?: Jenkins automates the CI/CD pipeline, facilitating the seamless integration, testing, and deployment of the application. It automates code quality checks, security scans, and Docker builds.  
  
Plugins: Various Jenkins plugins like SonarQube Scanner, OWASP Dependency-Check, and Docker tools are installed to enhance security and container handling.  
  
**7\. OWASP Dependency-Check**  
  
Why?: OWASP Dependency-Check scans the application for any known vulnerabilities in third-party libraries, ensuring that external dependencies are secure.  
  
**8\. Prometheus and Grafana**  
  
Why?: These tools are crucial for real-time monitoring.  
  
Prometheus monitors metrics like CPU usage, memory, and application performance.  
  
Grafana provides a visualization layer for the metrics collected by Prometheus, offering insights into the application’s health and performance.  
  
**9\. Helm and Kubernetes**  
  
Why?: Helm is used to install Node Exporter for monitoring the Kubernetes cluster, while Kubernetes is chosen as the container orchestration platform for scaling the application efficiently.  
  
**10\. ArgoCD**  
  
Why?: ArgoCD is used for deploying applications to the Kubernetes cluster, ensuring automated and declarative GitOps-style deployments.  
  
**11\. Notification Services (Jenkins Email Plugin)**  
  
Why?: These services are set up to send notifications about build failures, security vulnerabilities, or deployment statuses, keeping the team informed.  
  
Summary of the Project Phases:

#   
  
Phase 1: Development

  
  
Local Deployment using Docker  
Set up a local development environment for the Netflix clone using Docker to ensure platform consistency and easy containerization.  
  
[https://mysoftwarediary.hashnode.dev/phase-1-dev-local-deployment-using-docker  
  
](https://mysoftwarediary.hashnode.dev/phase-1-dev-local-deployment-using-docker￼￼Phase)

# [Phase](https://mysoftwarediary.hashnode.dev/phase-1-dev-local-deployment-using-docker￼￼Phase) [](https://mysoftwarediary.hashnode.dev/phase-1-dev-local-deployment-using-docker)2: Security

  
  
Implement Code and Image Scanning Tools (SonarQube and Trivy)  
Establish a security foundation by integrating SonarQube for code quality analysis and Trivy for container image vulnerability scanning.  
  
[https://mysoftwarediary.hashnode.dev/phase-2-security-setup-sonarqube-and-trivy  
](https://mysoftwarediary.hashnode.dev/phase-2-security-setup-sonarqube-and-trivy￼￼Phase)

# [  
Phase](https://mysoftwarediary.hashnode.dev/phase-2-security-setup-sonarqube-and-trivy￼￼Phase) 3: Operations  
  
Phase 3a: Jenkins Integration with SonarQube

  
Configure Jenkins to integrate SonarQube for automated code quality checks as part of the CI pipeline.  
  
[https://mysoftwarediary.hashnode.dev/phase-3a-ops-configure-necessary-tools-in-jenkins-integrate-sonarqube](https://mysoftwarediary.hashnode.dev/phase-3a-ops-configure-necessary-tools-in-jenkins-integrate-sonarqube￼￼Phase)

## [  
  
Phase](https://mysoftwarediary.hashnode.dev/phase-3a-ops-configure-necessary-tools-in-jenkins-integrate-sonarqube￼￼Phase) [3b: Jenkins In](https://mysoftwarediary.hashnode.dev/phase-3a-ops-configure-necessary-tools-in-jenkins-integrate-sonarqube)tegration with Trivy

  
Further configure Jenkins to integrate Trivy for vulnerability scanning of container images.  
  
[https://mysoftwarediary.hashnode.dev/phase-3b-ops-configure-necessary-tools-in-jenkins-integrate-trivy](https://mysoftwarediary.hashnode.dev/phase-3b-ops-configure-necessary-tools-in-jenkins-integrate-trivy￼￼Phase)

# [  
  
Phase](https://mysoftwarediary.hashnode.dev/phase-3b-ops-configure-necessary-tools-in-jenkins-integrate-trivy￼￼Phase) [4: Mo](https://mysoftwarediary.hashnode.dev/phase-3b-ops-configure-necessary-tools-in-jenkins-integrate-trivy)nitoring

##   
  
Phase 4a: Prometheus and Node Exporter Setup

  
Set up Prometheus along with Node Exporter to collect metrics and monitor the Netflix clone's infrastructure and application.  
  
[https://mysoftwarediary.hashnode.dev/phase-4a-monitoring-set-up-prometheus-and-grafana-to-monitor-your-application-prometheus-and-node-exporter-setup](https://mysoftwarediary.hashnode.dev/phase-4a-monitoring-set-up-prometheus-and-grafana-to-monitor-your-application-prometheus-and-node-exporter-setup￼￼Phase)

## [  
  
](https://mysoftwarediary.hashnode.dev/phase-4a-monitoring-set-up-prometheus-and-grafana-to-monitor-your-application-prometheus-and-node-exporter-setup￼￼Phase)Phase 4b: Grafana Setup for Application Monitoring

[  
](https://mysoftwarediary.hashnode.dev/phase-4a-monitoring-set-up-prometheus-and-grafana-to-monitor-your-application-prometheus-and-node-exporter-setup)Configure Grafana dashboards for visualizing the Prometheus metrics and monitor the application's performance metrics.  
  
[https://mysoftwarediary.hashnode.dev/phase-4b-monitoring-set-up-prometheus-and-grafana-to-monitor-your-application-setup-setup-grafana-monitor-node-exporter  
](https://mysoftwarediary.hashnode.dev/phase-4b-monitoring-set-up-prometheus-and-grafana-to-monitor-your-application-setup-setup-grafana-monitor-node-exporter￼￼Phase)

## [  
](https://mysoftwarediary.hashnode.dev/phase-4b-monitoring-set-up-prometheus-and-grafana-to-monitor-your-application-setup-setup-grafana-monitor-node-exporter￼￼Phase)Phase 4c: Jenkins Monitoring

[  
Set up Prometheus and Gr](https://mysoftwarediary.hashnode.dev/phase-4b-monitoring-set-up-prometheus-and-grafana-to-monitor-your-application-setup-setup-grafana-monitor-node-exporter)afana to monitor Jenkins performance, including job executions and [resource utilization.](https://mysoftwarediary.hashnode.dev/phase-4c-monitoring-monitor-jenkins-dashboard)[https://mysoftwarediary.hashnode.dev/phase-4c-monitoring-monitor-jenkins-dashboard  
](https://mysoftwarediary.hashnode.dev/phase-4c-monitoring-monitor-jenkins-dashboard￼￼Phase)

# [  
Phase](https://mysoftwarediary.hashnode.dev/phase-4c-monitoring-monitor-jenkins-dashboard￼￼Phase) 5: Notifications  

  
Configure Email Notifications  
Set up email notifications for build status updates, security issu[es, and monitoring alerts within Jenkins.  
  
](https://mysoftwarediary.hashnode.dev/phase-5-setup-email-notifications)[https://mysoftwarediary.hashnode.dev/phase-5-setup-email-notifications](https://mysoftwarediary.hashnode.dev/phase-5-setup-email-notifications￼￼Phase)

# [  
  
Phase](https://mysoftwarediary.hashnode.dev/phase-5-setup-email-notifications￼￼Phase) 6: Deployment

  
  
Deploy Netflix Clone Using ArgoCD and EKS  
Utilize ArgoCD for continuous deployment, managing Kubernetes resources [in AWS EKS for seamless Netflix clone deployment.  
  
](https://mysoftwarediary.hashnode.dev/phase-5-deploying-netflix-in-eks)[https://mysoftwarediary.hashnode.dev/phase-5-deploying-netflix-in-eks](https://mysoftwarediary.hashnode.dev/phase-5-deploying-netflix-in-eks￼￼Phase)

# [  
  
Phase](https://mysoftwarediary.hashnode.dev/phase-5-deploying-netflix-in-eks￼￼Phase) 7: Application Monitoring

  
  
Monitor Netflix Clone with Prometheus  
Set up and configure Prometheus to monitor the Netflix application metrics, ensuring a[vailability and performance optimization.  
  
](https://mysoftwarediary.hashnode.dev/phase-7-monitoring-netflix-app-using-prometheus)[https://mysoftwarediary.hashnode.dev/phase-7-monitoring-netflix-app-using-prometheus  
](https://mysoftwarediary.hashnode.dev/phase-7-monitoring-netflix-app-using-prometheus￼)