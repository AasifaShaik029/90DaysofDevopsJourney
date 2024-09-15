---
title: "Deploying an Angular App to AWS S3 using Terraform"
datePublished: Sun Sep 15 2024 10:29:24 GMT+0000 (Coordinated Universal Time)
cuid: cm13fp9ke00010alca24ze3br
slug: deploying-an-angular-app-to-aws-s3-using-terraform
cover: https://cdn.hashnode.com/res/hashnode/image/upload/v1726391487799/031ea9d6-d6cb-481a-8c09-cb5f714e8955.png
tags: angularjs, web-development, devops, terraform, s3, iac, s3-static-website-hosting

---

Hey there !! 🦥 In this blog, I will demonstrate how to automate the deployment of an Angular web application to AWS S3 using Terraform. By the end of this tutorial, you'll have a fully automated setup to push static files from your Angular project to an S3 bucket, making it accessible via an S3 website endpoint.

## Prerequisites

Before we begin, make sure you have the following tools installed:

1. **Terraform**: Install Terraform
    
2. **AWS CLI**: Install AWS CLI
    
3. **Angular Project**: You should have an Angular project built and ready to be deployed.
    
4. **AWS Account**: You'll need an active AWS account with appropriate permissions to create and manage S3 buckets.
    

Let’s quickly set the above prerequisites 🤸

( For Terraform and AWS Cli installation & configuration please refer appendix )

## Set Up Your Angular Project

Ensure your Angular project is built and ready for deployment. If you haven't built it yet follow the below steps.

Github repo : [AasifaShaik029/reactive-form-angular: Implementing reactive forms in angular (github.com)](https://github.com/AasifaShaik029/reactive-form-angular)

Let’s install node and angular CLI.

```plaintext
git clone https://github.com/AasifaShaik029/reactive-form-angular.git
cd reactive-form-angular
node -v
nvm install v20.17.0
npm install -g @angular/cli
ng version
npm install
npm outdated
npm update
npm install
ng build
ng serve
```

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1726394562031/60f0b8af-865a-4204-8619-ea3bf7a88753.png align="center")

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1726394603921/1d7b5571-26fc-4206-a8e9-c9e40fb3079d.png align="center")

🌷**ng build** command builds the application for production (or for deployment), generating optimized static files in the `dist/` directory. You can see dist folder is created once we run the ng build command.

🌷The `dist/` folder is crucial because it includes all the necessary files (HTML, CSS, JavaScript, and other assets) your application needs to run in a production environment. When you deploy your application, you'll typically upload the contents of this `dist/` folder to your server or hosting service.

🪻The `ng serve` command is used to start a development server for your Angular application. ( to check if app is running )

🪻 open http://localhost:4200/ to see the angular app running.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1726394955354/933605c2-536f-4c16-938a-52e0b3d011e6.png align="center")

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1726395078088/3438df27-cd3e-40f5-adf6-83dff57b44b5.png align="center")

## Initialize Terraform Configuration

Create a file called [`main.tf`](http://main.tf) and configure the necessary resources for your S3 bucket.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1726395412667/c1db03bd-4e23-466c-b416-7c09c53aa0fa.png align="center")

Here's the full Terraform configuration: ( main.tf )

```plaintext
provider "aws" {
  region = "eu-west-1"
}

variable "bucket_name" {
  default = "angular-reactive-form-002"
}

variable "mime_types" {
  default = {
    htm  = "text/html"
    html = "text/html"
    css  = "text/css"
    ttf  = "font/ttf"
    js   = "application/javascript"
    map  = "application/javascript"
    json = "application/json"
    ico  = "image/x-icon"
  }
}

# Use locals for the upload directory instead of variables
locals {
  upload_directory = "${path.cwd}/dist/app/browser/"
}

resource "aws_s3_bucket" "reactive_form" {
  bucket = var.bucket_name

  website {
    index_document = "index.html"
    error_document = "index.html"
  }
}

resource "aws_s3_bucket_public_access_block" "s3_public_block" {
  bucket = aws_s3_bucket.reactive_form.id

  block_public_acls       = false
  block_public_policy     = false
  ignore_public_acls      = false
  restrict_public_buckets = false
}

resource "aws_s3_bucket_ownership_controls" "s3_ownership" {
  bucket = aws_s3_bucket.reactive_form.id

  rule {
    object_ownership = "BucketOwnerPreferred"
  }
}

resource "aws_s3_bucket_policy" "allow_public_access" {
  bucket = aws_s3_bucket.reactive_form.id
  policy = data.aws_iam_policy_document.allow_public_access.json
}

data "aws_iam_policy_document" "allow_public_access" {
  statement {
    actions = ["s3:GetObject"]
    principals {
      type        = "AWS"
      identifiers = ["*"]
    }
    resources = [
      "arn:aws:s3:::${var.bucket_name}/*"
    ]
    effect = "Allow"
  }
}

resource "aws_s3_object" "website_files" {
  for_each      = fileset(local.upload_directory, "**/*.*")
  bucket        = aws_s3_bucket.reactive_form.bucket
  key           = replace(each.value, local.upload_directory, "")
  source        = "${local.upload_directory}${each.value}"
  acl           = "public-read"
  content_type  = lookup(var.mime_types, split(".", each.value)[length(split(".", each.value)) - 1], "application/octet-stream")
}

output "website_domain" {
  value = aws_s3_bucket.reactive_form.website_domain
}

output "website_endpoint" {
  value = aws_s3_bucket.reactive_form.website_endpoint
}
```

**Explanation of Code:**

```plaintext
Provider Block: Specifies AWS as the cloud provider and sets the region.
Variables: Defined for the S3 bucket name and file MIME types.
Local Block: We define upload_directory using path.cwd to point to the folder where the build files are located.
S3 Bucket Resource: Creates the S3 bucket with website hosting enabled.
Public Access Block: Ensures that the bucket is publicly accessible by disabling public access restrictions.
Ownership Controls: Ensures that ownership of objects is managed by the bucket owner.
S3 Object Resource: Uploads files from the build directory to the S3 bucket. The fileset function is used to read all files in the directory, and content_type is set dynamically based on file extensions.
Outputs: Displays the website domain and endpoint after deployment
```

Open the command prompt from the file location and execute the below commands.

```plaintext
# This will download the necessary provider plugins and prepare your workspace.
terraform init
terraform apply
# Terraform will show a preview of the resources it will create. Type yes to confirm the deployment.
terraform apply
```

## Access Your Angular App

You can now access your Angular app using the S3 website URL. The URL will be similar to the following format:

```plaintext
http://angular-reactive-form-002.s3-website.eu-west-1.amazonaws.com
```

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1726395923415/a1e27c6f-a38c-47bd-bb65-bc816140f793.png align="center")

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1726395904039/e420a8d0-ec45-4ac7-bf6f-9cbfa56d0a1e.png align="center")

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1726395834918/ace33d7e-b275-445e-820b-5ef16f087717.png align="center")

Under properties —&gt; static website hosting —&gt; copu URL

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1726395986849/72654d7a-b1b6-426e-b73b-8682bda125f2.png align="center")

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1726396013455/8d2fa5c5-4f4b-4f59-ae3b-82e7a114dc1c.png align="center")

In this tutorial, we used Terraform to deploy an Angular web application to an S3 bucket. This is a great way to automate your deployment process and ensure your app is always available via the S3 static site hosting. With the added advantage of using Terraform, you can now version control your infrastructure and collaborate with your team effortlessly.