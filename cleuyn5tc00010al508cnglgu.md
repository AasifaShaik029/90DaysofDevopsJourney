---
title: "Introduction to Ansible and Deployment of web app using ansible"
datePublished: Sun Mar 05 2023 05:36:11 GMT+0000 (Coordinated Universal Time)
cuid: cleuyn5tc00010al508cnglgu
slug: introduction-to-ansible-and-deployment-of-web-app-using-ansible
cover: https://cdn.hashnode.com/res/hashnode/image/upload/v1677994530090/664cee70-5c69-4da5-92dd-3506295a4ca5.jpeg
ogImage: https://cdn.hashnode.com/res/hashnode/image/upload/v1677994559842/a458e1a5-1e4f-49d8-90f3-d59b7c2d8484.jpeg
tags: aws, ansible, devops

---

### Infrastructure as a Code Vs Configuration Management

### **IAC**

It refers to the process of managing and provisioning the infrastructure such as servers, networks, and storage, through code without manual effort.

what does infrastructure include?

Primarily focused on defining and managing software and virtual infrastructure components such as virtual machines, containers, networks, and virtualized storage infrastructure.

IAC is writing code. This code is then used to automatically provision and configure the infrastructure. Code can be written in a variety of programming languages, such as YAML, JSON, or Python, and can be managed using version control tools such as Git.

Examples of IAC tools : Ansible, Terraform, and CloudFormation

### **Configuration Management**

Configuration management is the process of managing and maintaining the configuration of software and systems, including the hardware, software, and network components.

Configuring refers to the process of setting up and defining the options, and parameters that define how software is arranged to operate within an IT system.

It includes what services are started, what are services installed, what needs to be backup, giving permissions to the user, adding users, etc.

Examples of Configuration Management tools: Ansible, Puppet, chef

Note: Terraform is an IAC tool whereas ansible is a tool that manages this IAC.

IAC and Configuration Management tools can help organizations to improve the efficiency, reliability, and security of their IT infrastructure while also making it more agile and scalable.

If we use IAC, then without any manual effort, we can automate things.

Important terminologies:

Provisioning: Setting up the infrastructure.

---

### Ansible in real-time

Previously, system ops used to maintain a configuration file, which will have all the details of all the servers/operating systems. In order to update, check status, manage package management, backup, rollback, set up software, etc system ops are used do it. which is tedious.

Here comes ansible where with one master server, everything will be configured to all the servers.

### Approaches used in Configuration Management

1. push Configuration management (Ansible)
    
2. Pull Configuration management (chef and puppet)
    
    ![](https://cdn.hashnode.com/res/hashnode/image/upload/v1677819810706/ca6fd3a8-8142-4421-9532-ad3345feb36b.png align="center")
    
    ### **push Configuration management:**
    
    Master Node "pushes" the updates out to the worker nodes.
    
    Master Node decides what configuration changes need to be made and then pushes those changes to the worker nodes.
    
    ### **Pull Configuration management:**
    
    The configuration changes are initiated by the worker nodes themselves.
    
    The worker nodes regularly check a central management server (Master Node) or a configuration management tool for updates and then "pull" those updates down to their local systems. Its an agent-based.
    
    Let's learn about push Configuration management through Ansible.
    
    ### Ansible
    
    This is a master and worker node architecture.
    
    Things that need to be installed/should have in Master Node to connect with worker nodes are:
    
    1. Ansible installed
        
    2. Should have an IP address and username of worker nodes.
        
        (No need of installing ansible in worker nodes 😉 )
        
    3. Should have SSH key
        
    
    ### Hands-On
    
    Let's do Hand-On now 😀
    
    Create EC2 Instances ( for Master and Worker Nodes)
    
    While creating use the same key pair.
    
    Use of private and public keys of Master Server:
    
    The master node's public key will be shared with Worker Nodes.
    
    The master node's private key will be present in a master node which is used to connect with worker nodes by sharing with them.
    
    ### Installing Ansible in Master Node
    
    Follow [Ansible Tutorial - Google Docs](https://docs.google.com/document/d/1aepYrnL38oN5_LCvmJFY_Q-ez3iExQHq98XCUzeW3NE/edit) (Ansible is built in python)
    
    ```plaintext
    # installing req libraries needed to install ansible
    sudo apt-add-repository ppa:ansible/ansible
    ```
    
    ```plaintext
    sudo apt update
    ```
    
    ```plaintext
    sudo apt install ansible -y
    ```
    
    ```plaintext
    # To check if ansible is set up or not
    cat /etc/ansible/hosts
    ```
    
    you should see a file like this, which means ansible is installed.
    
    ![](https://cdn.hashnode.com/res/hashnode/image/upload/v1677827945253/ee183a2b-0acd-49b1-960c-59850f8413f1.png align="center")
    
    This is called an Inventory file where all details of the servers will be present.
    
    Create 3 (let's take) worker nodes with the same access key used. Collect all their IP addresses and add them to this inventory.
    
    ![](https://cdn.hashnode.com/res/hashnode/image/upload/v1677829429601/bfc65403-4581-4e0d-86d3-e56da143f896.png align="center")
    
    ![](https://cdn.hashnode.com/res/hashnode/image/upload/v1677829612876/3b5037d2-c4ef-4143-8a11-173d09fb6335.png align="center")
    
    Let's connect with worker nodes. Before that add a private key (pem file of the master node ) from the local machine to the master node server. 👇
    
    ```plaintext
    # secure copy scp is used to copy files from local to remote. Here we are cpoying pem file that was downloaded in the system to master node in /home/ubuntu/.ssh folder.
    scp -i "Ansible-Access-Key.pem" Ansible-Access-Key.pem ubuntu@ec2-13-232-231-99.ap-south-1.compute.amazonaws.com:/home/ubuntu/.ssh
    ```
    
    local system:
    

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1677839472509/3a65b022-7cab-49c5-b2b8-9477eabbc95b.png align="center")

remote master node: you can see that pem file was copied into master node.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1677839515676/1afc034d-7405-4f9b-b4bd-0f3f95aa25ae.png align="center")

You need to use this private pem file of master node in worker nodes to connect.

This pem file should be stored in inventory file (shown above) and this will be allocated to a variable and it will be configured to worker nodes. Lets see.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1677840406189/a36e8ed9-3306-49c1-941c-ae0e299b61af.png align="center")

```plaintext
ansible_python_interpreter=/usr/bin/python3
ansible_ssh_private_key_file=/home/ubuntu/.ssh/Ansible-Access-Key.pem
```

Give permission to pem file

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1677840670354/43e53ba1-f615-48f7-b40f-1f5575b05ad9.png align="center")

```plaintext
# used to connect to the worker nodes
ansible servers -m ping
```

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1677840818906/223eb0f0-32be-4a60-867c-c5ead133132e.png align="center")

Now we worker nodes were connected with master. 😀

Lets check the disk space of our worker nodes using master node.

```plaintext
ansible servers -a "df -h"
```

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1677841032245/0b0a6f7a-b527-4294-8258-11fb21574529.png align="center")

It listed disk space of all the 3 worker nodes. 😇

Likewise you can update , install anything in worker nodes without actually going inside worker nodes, just simply by using master node.

```plaintext
# It will update all the servers 
ansible servers -a "sudo apt update"
# It will display running details of the servers
ansible servers -a "uptime"
# it will create dir in all the servers
ansible servers -a "mkdir ansible-dir"
```

---

### Multi Environment Ansible Configuration

In real time you will be having different environments like prod, dev, stage. For each environment there should be a inventory file. Give prod application ipaddress in Inv file.

```plaintext
ansible -i prod-inv servers -m ping
```

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1677908819290/964793b1-8fb2-439e-b647-92f2da42ed08.png align="center")

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1677908848951/13f35d46-7919-4704-9302-4624c7dcae5b.png align="center")

---

### Playbook

A playbook is a set of instructions that defines a standardized process for executing a specific task or workflow.

For example, a DevOps playbook might outline the steps for deploying a new application or updating an existing one.

Playbook name can be anything. It is an YAML file.

```plaintext
# - is list of tasks
# The playbook is name is  "Date playbook"
# runs of group of servers as defined above
# This playbook consists of task which will run on each of the servers, the task is displaying the date and time using date command
-
 name: Date playbook
 hosts: servers
 tasks:
   - name: this will show the date
     command: date
```

```plaintext
# Run playbook using 
ansible-playbook date.yaml
```

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1677927630347/59db6aa1-2dcf-476a-89ae-5703b2ca8b7e.png align="center")

Now let's install nginx in all the servers using playbook.

```plaintext

# This playbook will install nginx and start nginx
# become : yes is giving sudo permission to install 
-
 name: Install nginx
 hosts: servers
 become: yes
 tasks:
   - name: nginx Installer
     apt:
       name: nginx
       state: latest 
   - name: nginx start
     service:
        name: nginx
        state: started
        enabled: yes
```

```plaintext
# run the playbook above using
ansible-playbook install-nginx
```

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1677930017849/4bcf3cdf-de52-4300-bf02-d94e9b5711c6.png align="center")

Let's see if nginx is installed in the servers or not.

It is one of the worker nodes and ngnix is installed. 😉

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1677930124086/1c3aed13-3aa4-4f4f-a14a-a47f494ff9d9.png align="center")

Note : you can use condition to install software. like below, only when OS is debian or ubuntu then install apache else it will skip.

```plaintext
- 
  hosts: servers
  become: true

  tasks:
  - name: Install apache
    apt: 
      name: apache2
      state: latest
    
    when: ansible_distribution == 'Debian' or ansible_distribution == 'Ubuntu'
```

with\_items in ansible, instead of writing command to install multiple software, use with\_items as loop it will install below softwares.

```plaintext
---
- name: Install Mysql package
  yum: name={{ item }} state=present
  with_items:
   - mysql-server
   - MySQL-python
   - libselinux-python
   - libsemanage-python

- name: Configure SELinux to start mysql on any port
  seboolean: name=mysql_connect_any state=true persistent=yes
  when: ansible_selinux.status == "enabled"

- name: Create Mysql configuration file
  template: src=my.cnf.j2 dest=/etc/my.cnf
  notify:
  - restart mysql

- name: Start Mysql Service
  service: name=mysqld state=started enabled=yes
```

You can use for loop as well

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1677945471347/ea4d2bdd-edd3-4c00-a1dc-6d3548c71d97.png align="center")

---

### Deploying application using ansible

Now lets deploy one simple HTML application using Ansible. Maintain evrything in a single directory. This application should deploy in prod-inv inventory that we created above.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1677942402654/5e7d67cc-268c-49a1-9d18-e1e75823d7ae.png align="center")

**index.html**

```plaintext
<!DOCTYPE html>
<html>
  <head>
    <style>
      body {
        font-family: Arial, sans-serif;
        background-color: #f2f2f2;
        color: #333;
        text-align: center;
      }

      h1 {
        font-size: 36px;
        margin-top: 50px;
        color: #6130e8;
      }

      p {
        font-size: 18px;
        margin: 20px 0;
      }
    </style>
  </head>
  <body>
    <h1>Welcome to Ansible</h1>
    <p> *** App deployed successfully using ansible :) *** </p>
  </body>
</html>
```

**deploy\_webpage.yaml**

```plaintext
-
 name: this is a  simple html project
 hosts: servers
 become: yes
 tasks:
   - name: Install nginx
     apt:
       name: nginx
       state: latest

   - name: Start nginx
     service:
       name: nginx
       state: started

   - name: Deploy webpage
     copy:
       src: index.html
       dest: /var/www/html
```

```plaintext
# deploy the app using below command
ansible-playbook -i inventories/prod-inv deploy_webpage.yaml
```

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1677942048587/d65b5e0f-91bc-4cae-b0a2-26c9d7bb057d.png align="center")

Check the prod server using, it will list all the servers present inside prod environment. Our app is running in this IP address. 😊🤩

```plaintext
ansible-inventory -i inventories/prod-inv --list -y
```

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1677942783400/c9fcedfe-def6-421b-bb43-3bd27741e551.png align="center")

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1677942832563/69f2ef67-afa1-4730-8ea8-69d60675e885.png align="center")

---

### Ansible Tower

Ansible Tower is a web-based graphical interface and automation management platform that provides a centralized view of Ansible playbooks, inventory, and job runs. It is developed and maintained by Red Hat.

---

\--Thank you for your time--

Happy Learning 😊