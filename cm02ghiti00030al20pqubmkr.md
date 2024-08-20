---
title: "AWS Certified Solutions Architect – Associate (SAA-C03) Notes - Part 2"
datePublished: Tue Aug 20 2024 13:23:53 GMT+0000 (Coordinated Universal Time)
cuid: cm02ghiti00030al20pqubmkr
slug: aws-certified-solutions-architect-associate-saa-c03-notes-part-2
tags: aws, aws-certified-solutions-architect-associate, aws-saa-c03-dumps

---

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1724160135736/56f67479-625a-4232-a918-bae0e17242df.png align="center")

### Elastic Load Balancer (ELB)

* **Sticky Sessions**: ELB's Sticky Session feature ensures that traffic for the same client is always redirected to the same target (e.g., EC2 instance), preventing session data loss.
    

### Amazon Aurora

* **Global Databases**: Aurora Global Databases allow you to have an Aurora Replica in another AWS Region, with up to 5 secondary regions.
    
* **IAM Auth**: IAM authentication is not supported for ElastiCache Redis; it is supported for RDS MySQL and RDS PostgreSQL.
    
* **Encrypted Read Replicas**: You cannot create encrypted Read Replicas from an unencrypted RDS DB instance.
    

### Amazon Machine Images (AMI)

* **Golden AMI**: A Golden AMI is an image that contains all your software, dependencies, and configurations, allowing future EC2 instances to boot up quickly from that AMI.
    

### Elastic Beanstalk

* **Cost-Efficient Deployment**: For minimal cost in the development stage, use Single Instance Mode, which creates one EC2 instance and one Elastic IP.
    

### Amazon S3 Security

* **Client-Side Encryption**: You handle encryption yourself and have full control over the encryption keys. AWS does not know your encryption keys and cannot decrypt your data.
    
* **SSE-KMS**: AWS handles encryption, but you have control over the rotation policy of the encryption keys. The keys are managed and stored by AWS.
    
* **SSE-C**: Encryption happens in AWS, but you fully manage the encryption keys, and AWS does not store them. Recommended when the client wants to manage keys and avoid storage in AWS.
    
* **CORS**: Cross-Origin Resource Sharing (CORS) allows client web applications in one domain to interact with resources in another domain. [Learn more about CORS](https://docs.aws.amazon.com/AmazonS3/latest/dev/cors.html).
    

### AWS Global Accelerator

* **Static IP and Low Latency**: AWS Global Accelerator provides static IP addresses and directs traffic from users to applications across multiple AWS regions, optimizing network paths.
    

### AWS Transfer Family

* **File Transfers**: Managed service for file transfers to/from S3 or EFS using the FTP protocol. Note that TLS is not supported.
    

### AWS DataSync

* **Data Transfer**: Simplifies and accelerates transferring data between on-premises storage and AWS storage services, useful for migration, content distribution, disaster recovery, and archival.
    

### Amazon FSx for NetApp ONTAP

* **Protocols**: Compatible with NFS (v3 and v4.1) and SMB (v2.1 and v3.0). Not compatible with FTP.
    

### Amazon ElastiCache

* **Memcached**: Does not support geospatial data.
    

### AWS Snowball

* **Data Transfer**: You cannot directly copy data from Snowball Edge devices into AWS Glacier. Data must be moved from Snowball to an S3 bucket before archiving to Glacier.
    

### Integration and Messaging

* **Synchronous Communication**: Involves real-time interaction where the sender waits for an immediate response from the receiver.
    
* **Asynchronous Communication**: Allows decoupling, where the sender and receiver operate independently without needing to be synchronized in real-time.
    

---