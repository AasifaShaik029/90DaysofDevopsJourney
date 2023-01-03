# Day 1 : Getting started with DevOps

Hi Everyone! :)

My name is Aasifa. I have started my DevOps Journey and would like to share my journey through the articles.

I am following TrainWithSubham DevOps Bootcamp.

Here is the Day 1 content which covers what DevOps is and Tools used for in it.

**What is DevOps?**

DevOps is a methodology that bridges the gap between Developers and the Operation team. Enables the organization’s ability to deliver applications and services at high velocity

**Concept of Continuous Integration and Continous Delivery/Deployment**

SDLC consists of

Code ----&gt; Build -----&gt; Test ------&gt;Deploy

Unit tests                                   integration tests

Dev--------------------&gt;Testing---------------&gt;Deployment(staging env)

**Continuous Integration:**

It mostly happens in the dev phase. It is about pushing the code. Every day, the dev team will continuously integrate the code and push the code in version control systems like GIT(shared repository).

**Continuous Delivery:**

Making it available for deployment. You won't deploy directly into the production server, instead deliver to the mock servers(staging servers) and make deployment ready.

**Continuous deployment:**

Automated deployment to production. It is deploying into the production server (the final step which is risky if there are any faults). Every Change that passes through the automation tests will be deployed into the production server.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1671862109199/ff46aec8-9ca9-4ff3-a326-b683aea4dae3.png align="center")

**Tools needed to learn:**

1. Linux (shell scripting)
    
2. Source Code Management (Git and GitHub)
    
3. Docker and Docker Swarm (Creates virtual environment)
    
4. Kubernetes and Helm (Handle the containers)
    
5. Grafana