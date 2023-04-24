---
title: "( 5 ) Amazon RDS - Fully Managed Relational Database"
datePublished: Fri Apr 21 2023 14:09:03 GMT+0000 (Coordinated Universal Time)
cuid: clgqmnqo7000509mlh7pe3qrv
slug: 5-amazon-rds-fully-managed-relational-database
cover: https://cdn.hashnode.com/res/hashnode/image/upload/v1681273072905/6ad869dd-4fca-4bbd-9134-ee056a86ed15.png
tags: aws, devops, aws-rds

---

### RDS

RDS (Relational Database Service) is a managed service provided by Amazon Web Services (AWS). Generally, Relational Database organizes data into one or more tables with a predefined structure, where each table consists of rows and columns. The data in these tables is related to each other using keys, which establish a relationship between the tables.

Example : Banking systems: Banking systems use relational databases to store customer information, account balances, transaction histories, and other financial data. For example, a bank might have tables for customers, accounts, transactions, and ATM locations, with relationships between the tables to track money flows and user activity.

### RDS - A fully managed service by AWS

Amazon Web Services (AWS) manages several aspects of RDS to make it easier for customers to set up, operate, and scale their databases in the cloud. Such as,

1. Infrastructure:
    
    AWS manages the underlying infrastructure, including servers, storage, networking, and security, so that customers don't have to worry about managing these components themselves.
    
2. Software updates and patching:
    
    Patching in RDS terms, patching refers to updating the database software to the latest version that includes bug fixes, security patches, and other improvements. AWS automatically applies software updates and security patches to the RDS instances, ensuring that customers always have access to the latest version of their database software.
    
3. Backup and recovery:
    
    AWS automatically performs backups of the RDS database instances according to customer-defined backup policies and also enables customers to perform manual backups and point-in-time restores as needed.
    
4. High availability and failover:
    
    AWS provides multiple availability zones for RDS instances, which enables automatic failover in case of a hardware or software failure, ensuring that customers have continuous access to their databases.
    
5. Monitoring and metrics:
    
    AWS provides a range of monitoring and metrics tools for RDS, including CloudWatch and Performance Insights, which enable customers to monitor their database performance and troubleshoot issues.
    
6. Security:
    
    AWS provides a range of security features for RDS, including network isolation, encryption, and access control, to ensure that customer data is secure and protected.
    

### What customer needs to manage?

1. Managing database settings
    
2. Creating a relational database schema
    
3. Database performance tuning (database performance tuning in RDS is a continuous process that involves monitoring, testing, and adjusting various parameters to achieve the best possible performance for the application. )
    
    ### Different relational database options available in RDS
    
    1. MS SQL Server
        
    2. My SQL
        
    3. Oracle
        
    4. AWS Aurora (high throughput)
        
    5. PostgreSQL (stable and reliable )
        
    6. Maria DB (MySQL compatible )
        
    
    ### RDS Limits
    
    1\. Upto 40 DB instances you can create per account
    
    1. 10/40 can be Oracle or MS- SQL server under was a license
        
    2. 40/40 can be all databases under BYOL (bring your own license )
        
    
    ### Instance storage
    
    Generally, databases will be created in EC2 but we don't have access to ec2. Only have access to a database that is created on ec2. And the rrot volume of the ec2 instance is **EBS** volume.
    
    Storage options
    
    1. General purpose
        
    2. Provisioned IOPS
        

### Concept of Multi-AZ

Inorder to provide fault tolerance and high availability.

You need to select the Multi-AZ option if you want to create a standby/replica of database. Multi-AZ helps when you plan a disaster recovery for an entire AZ going down. If you plan against an entire AWS Region going down, you should use backups and replication across AWS Regions.

When you are creating a database you can either choose standalone or standby (replica of the database in case of failure). **Standby will be made in another availability zone of the same region.**

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1681456556571/ed156421-aef4-4cfe-8365-700613c105bc.png align="center")

You can see above that we have one Master database in one AZ. If we go by standby , another replica of the database will be created in another AZ of the same region. Whatever changes done in the master will also synchronously be reflected in the replica. If the Master database fails, then replica/standby becomes Master.

This is called Automatic failover recovery.

\--&gt; You can select multi-AZ option when you are creating a database.

\--&gt; You cannot create a standby in one particular AZ, AWS will automatically creates a standby in AZ.

### When Multi-AZ RDS failover triggers? (when it will move to standby?)

1. When your primary db instance fails.
    
2. When AZ fails
    
3. loss of network connection
    
4. EBS of primary instance is fails
    
5. Patching the OS of the DB instance.
    
6. Manual failure of primary ( when you are doing maintenance of primary)
    
    ### The difference between Read Replicas and Multi-AZ
    
    Read replicas and Multi-AZ (Availability Zone) are two different mechanisms used in database replication to improve data availability and durability in Amazon Web Services (AWS) environment.
    
    Read Replicas: These are the copies of the primary database. Read replicas are called "read" replicas because their primary purpose is to handle read requests from the application, while the primary database handles write requests. changes will be asynchronously replicated. With asynchronous replication, changes made to the primary database are asynchronously propagated to the read replicas. This means that the primary database does not wait for confirmation that the changes have been made to the read replicas before proceeding with the next operation. However, read replicas are not designed for failover or disaster recovery, and they do not provide automatic failover or recovery.
    
    \--&gt;Multi-AZ is a feature that provides high availability for the primary database by automatically replicating the data to a standby database in a different Availability Zone (AZ) within the same region. Multi-AZ is designed to provide automatic failover and data durability, ensuring that your database remains available even if the primary database becomes unavailable.
    

### Hands-On

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1681973843146/0e6d0877-eb5a-48a8-bed9-16226397ca46.png align="center")

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1681977001291/13d734a9-44fb-4853-b00b-f05312b26de6.png align="center")

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1682075262398/b22ba137-589f-4710-884e-062a6a8dbe0f.png align="center")

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1682075576498/71dd6406-4eac-4d28-a670-d54bd6c1d6c3.png align="center")

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1682075631307/014af42a-63d9-4d1a-921d-23c07d580180.png align="center")

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1682075713749/eb6edb61-8356-4e1e-bc53-5e40b44deb23.png align="center")

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1682075766884/fab3fcd1-a1ef-46b5-8844-3b8ddb9e8d0e.png align="center")

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1682075836369/cf703c23-fbe1-4583-8043-221482a68ed7.png align="center")

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1682075902845/dcdb32e3-ca44-43b5-a1e9-487a1b25219e.png align="center")

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1682075930627/ded45d5d-c60d-4748-b687-2f383d829855.png align="center")

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1682075984128/375f1f30-7efd-493f-8de7-2cafd7f6ab39.png align="center")

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1682076814672/496f1158-65ca-4246-8187-077bd908ab87.png align="center")

Change the SG to 3306 to anywhere.

### Connecting using SQL electron

To connect the database i will use SQL electron.

[Internet - Servers - Database Utils Downloads (](https://www.softpedia.com/get/Internet/Servers/Database-Utils/?utm_source=spd&utm_campaign=postdl_redir)[softpedia.com](http://softpedia.com)[)](https://www.softpedia.com/get/Internet/Servers/Database-Utils/?utm_source=spd&utm_campaign=postdl_redir)

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1682077501529/24df46dc-2265-427d-aef7-e9ddbb14fb72.png align="center")

Copy endpoint URL of your database

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1682077836360/03901ab2-239b-4378-9b33-2b883c83a139.png align="center")

Click on Test

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1682077861695/d3f6b5d3-2a1e-4aa8-9b15-adce14a21aa1.png align="center")

Save and connect to the db

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1682077896963/7c2dc3b5-15b2-49e9-beb1-7bfe6d1c6dda.png align="center")

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1682077915917/71da8ee3-d9e5-4df5-8c08-82ce0f88b0c8.png align="center")

Lets create a Table here 😄

```plaintext
CREATE TABLE students (
    PersonID int,
    LastName varchar(255),
    FirstName varchar(255),
    Address varchar(255),
    City varchar(255)
);

INSERT INTO students (PersonID, LastName, FirstName, Address, City)
VALUES (001,'Aasifa', 'Shaik', 'Hyderabad', 'telangana');
```

You can insert the data and perform other operations

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1682078848223/71ac6c73-e43d-4642-895a-92e91e87a567.png align="center")

### Connecting using EC2 instance

While creating select SG that we created for RDS database.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1682085012732/231b6e5e-dee2-4fd9-8648-be106f818616.png align="center")

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1682084721254/397198d4-0d5a-4679-a406-99a058208aad.png align="center")

Give following commands to update, install MySQL and to connect to the server.

```plaintext
       sudo apt-get update 
       sudo apt install mysql-client-core-8.0
#give username as admin, and databasename
       mysql -h database-1.cicv6luoqc5p.ap-south-1.rds.amazonaws.com -u admin -p myfirstdb
```

Now our RDS DB is successfully connected and you can access it via EC2😃😌👍

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1682085389909/94f95dee-fa4a-4e8b-a4b1-717348076b65.png align="center")

---

I hope you learnt about RDS. Follow more for such articles. Thank you for your time 🌈🌈🌈🌈🌈🌈🌈🌈🌈🌈🌈🌈🌈🌈🌈🌈🌈🌈🌈🌈🌈🌈🌈🌈🌈🌈🌈🌈