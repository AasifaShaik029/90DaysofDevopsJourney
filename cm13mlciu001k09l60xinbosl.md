---
title: "AWS CLI Installation and Configuration"
datePublished: Sun Sep 15 2024 13:42:18 GMT+0000 (Coordinated Universal Time)
cuid: cm13mlciu001k09l60xinbosl
slug: aws-cli-installation-and-configuration
cover: https://cdn.hashnode.com/res/hashnode/image/upload/v1726407281728/6f1497f3-3e73-49f5-aa35-29428315ff65.jpeg
tags: aws-cli, install-awscli

---

[The AWS CLI](https://mysoftwarediary.hashnode.dev/terraform-installation-step-by-step) is needed to interact with AWS services from your terminal. Here’s how to install it:

**Download AWS CLI**:

1. Visit the [Install or update to the latest version of the AWS CLI - AWS Command Line Interface (amazon.com)](https://docs.aws.amazon.com/cli/latest/userguide/getting-started-install.html)
    
2. Download the Windows installer (`.msi` file) as shown below.
    
    ![](https://cdn.hashnode.com/res/hashnode/image/upload/v1726406179155/b4dd85dd-650b-4c59-acda-6ce796f3ac40.png align="center")
    
    3. Run the installer and follow the on-screen instructions.
        
    4. After installation, open a new Command Prompt and type `aws --version` to check if AWS CLI is installed.
        

### **Configure AWS CLI**

1. Run `aws configure` in the Command Prompt.
    
2. Enter your AWS credentials (Access Key ID and Secret Access Key), along with the default region (e.g., `us-east-1`).
    
    ```plaintext
    aws configure
    AWS Access Key ID [None]: YOUR_ACCESS_KEY
    AWS Secret Access Key [None]: YOUR_SECRET_KEY
    Default region name [None]: us-east-1
    Default output format [None]: json
    ```
    
    For Access key and secret key ,
    
3. Navigate to the IAM (Identity and Access Management) Dashboard.
    
4. Create a New IAM User (if needed).
    
5. Attach Permissions.
    

In the next step, you will need to attach permissions to the new user. Choose one of the following options:

* Select **Attach policies directly** and choose **AmazonS3FullAccess** (for S3 access) and any other required policies.
    
* Alternatively, select **AdministratorAccess** if you want the user to have full AWS account permissions.
    

6. Get the Access Key and Secret Key under security credentials.
    
7. ![](https://cdn.hashnode.com/res/hashnode/image/upload/v1726407380669/02a72cbf-779e-4d44-b208-b0408e274734.png align="center")
    
    ![](https://cdn.hashnode.com/res/hashnode/image/upload/v1726407452509/0a9e325e-ef1f-4208-ab60-c97e32387c95.png align="center")
    
    ![](https://cdn.hashnode.com/res/hashnode/image/upload/v1726407486718/146d979d-10b3-4547-a35a-88c7da2da593.png align="center")
    
    ![](https://cdn.hashnode.com/res/hashnode/image/upload/v1726407515390/c225ef9f-e307-46f3-9789-294d02f036df.png align="center")
    
    **Important**: Save these keys! You will only be shown the **Secret Access Key** once, so copy and store them in a secure place.
    
    You can also download the credentials as a `.csv` file by clicking the **Download .csv** button.
    
8. Enter the following details when prompted:
    
    * **AWS Access Key ID**: Your Access Key ID from the IAM Dashboard.
        
    * **AWS Secret Access Key**: Your Secret Access Key from the IAM Dashboard.
        
    * **Default region name**: e.g., `us-east-1` (or whichever region you prefer).
        
    * **Default output format**: You can choose `json`, `text`, or `table` (default is `json`).
        
        ![](https://cdn.hashnode.com/res/hashnode/image/upload/v1726407653097/e6041fee-61ba-4a25-9677-0cf9df21fe7e.png align="center")
        
        AWS Configuration is done. 💁‍♀️