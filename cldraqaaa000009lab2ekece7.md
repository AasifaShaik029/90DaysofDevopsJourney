# Day 11: Jenkins Agents

Hello Everyone!

I learnt about agents in jenkins. Please find my below notes. Hope it will be useful

### Agents

In Jenkins, an agent refers to a machine that is set up to run build jobs. An agent can be a physical machine or a virtual machine, and it is managed by the Jenkins master server.

Let's say I have a master Jenkins server with 8 GB has 3 jobs. Example Django , Node js , Java application. Each application requires 10 gb, 30 gb and 50 gb. These applications cannot be run in the master server as it is only has 8 gb.

This master server is linked with 3 agents with 16 gb , 32gb, 64 gb each. As master cannot run those 3 jobs agents will run the jobs based on requirement.

Agent 1

Agent 2

Agent 3

These are labels

All the jobs will be present in the master. Agents can connect to the master thru SSH.

In order to connect with agents, java should be installed in these agents.

Master will allocate the agents to run the jobs.

### Steps

1. To connect agent with the master, we need to first Add the credentials.
    
    Go to credentials and add private key of master jenkins server in it and save.
    
2. Make sure that agent server is having java installed in it to connect.
    
3. In the agent server also add public key of the master jenkins server.
    
4. Configure the agent node by giving agent ip address, lable to be identified with,root home directory (like home/ubuntu/jenkins-agent-server)this will automatically be created in jenkins agent server. Non verification strategy.
    
5. Launch the agent to connect
    
6. When you are trying to deploy the application in master jenkins, then agent will deploy the application. So make sure to install all the necessary softwares installed in the agnet server.
    

### Setting up agent in Jenkins

Dashboard - Manage Jenkins - Nodes - New node

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1675409345834/53d08601-b6ef-47c8-96bf-5f2216f44c8e.png align="center")

I have a master instance (Jenkins server). I will create a new instance (agent -1).

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1675409799574/f9431471-470e-4e80-90b5-1df9f5c7b82c.png align="center")

### Connecting agent with master

**Creating agent in master jenkins**

To connect agent with master, you need to ssh.

In master we will generate ssh keys which consists of private key and public key.

Public key will be given to agent

private key will be in master.

**In master server:**

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1675410455384/d13f92e8-e05d-46e3-83b4-9e4fd752b254.png align="center")

id\_rsa -- private key

id\_rsa.pub -- public key

**agent-1**

We will add public key of master in agent-1 authorized keys.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1675410796345/db023985-095f-4822-a9ef-080722459b2b.png align="center")

Add this below public key in the above file

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1675410879756/9222946f-c3db-414a-8c2b-b5cbbb9e088e.png align="center")

added public key of master in agent-1 in vim authorized keys file.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1675411044844/41cafab3-8d6d-4dea-a635-9fd0a34faaa9.png align="center")

open jenkins and

👉 give agents ipaddress as host

👉give private key of master (because jenkins is running on master)

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1675413352095/2d8668c3-2caa-4493-8fa2-a84c52f2c16c.png align="center")

Add master private key in jenkins. Host name is agents IP address

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1675416490489/50eba822-a4e4-4e65-9656-f8c5b26d7ffb.png align="center")

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1675416693748/7411eac8-ab48-4779-a552-29fbd7d396d8.png align="center")

We wont be able to connect to the agent because there is no java installed in agent1.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1675416647476/563da96a-5b98-48d7-bfc2-23b4871ce594.png align="center")

Install java in agent1 and launch agent

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1675594744940/edd31a60-a2d3-4f69-a697-c2f66290e940.png align="center")

### Agent deploying app

Lets try deploying application in the agent.

I created a node to do app job.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1675594940027/3f2a0125-5214-4030-bf29-0e44ec6c3f75.png align="center")

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1675594896334/e10305c1-f1da-4082-b366-f70ea13c5ad5.png align="center")

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1675594954409/3c32e7de-1113-422e-b59c-7a47d1d51530.png align="center")

Make sure that docker, docker compose are installed in agent server.

now build the application.

you can see that this application is handled by the agent.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1675595061286/aad9756b-f970-4113-89f8-2506d846e944.png align="center")

You can see that our app is now present in agent server.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1675595276418/fddea39f-3376-468a-927a-61fbcfeddb97.png align="center")

---

\--Thank you for your time. Happy Learning--