---
title: "Understanding and Resolving the "AccessControlListNotSupported" Error"
datePublished: Fri Sep 20 2024 09:03:11 GMT+0000 (Coordinated Universal Time)
cuid: cm1ahtnn5001a0alf4cl58aoq
slug: understanding-and-resolving-the-accesscontrollistnotsupported-error
cover: https://cdn.hashnode.com/res/hashnode/image/upload/v1726822947723/abeea5b6-a54a-4e7d-8044-cbc340f4ee09.jpeg
tags: accesscontrollistnotsupported, s3error

---

While deploying my Angular app to an S3 bucket using Terraform, I encountered the following error:

**Error:** `AccessControlListNotSupported: The bucket does not allow ACLs`

This error occurs when an S3 bucket has **BucketOwnerEnforced** ownership, meaning that AWS no longer supports the use of Access Control Lists (ACLs) for such buckets. ACLs have traditionally been used to control access to objects in S3, but with modern S3 configurations, AWS recommends managing permissions using **Bucket Policies** or **IAM Policies** instead of ACLs.

#### **Why This Error Happens**

The S3 bucket ownership model **BucketOwnerEnforced** enforces that the bucket owner is the only one responsible for object permissions, rendering ACLs irrelevant. Hence, if you attempt to apply any ACL-related settings to such a bucket, you’ll encounter the **AccessControlListNotSupported** error.

#### **How I Resolved the Error**

To fix this issue, I made the following adjustments to my [`main.tf`](http://main.tf) file:

1. **Removed ACL settings**: I completely removed references to ACLs from my Terraform configuration.
    
2. **Set Ownership Controls**: I added ownership controls for the S3 bucket by setting `object_ownership` to `BucketOwnerEnforced` to explicitly state that no ACLs will be used.
    
3. **Updated Public Access Block Settings**: I modified the `aws_s3_bucket_public_access_block` settings to block any public ACLs but still allow public policies.
    
4. **Used a Bucket Policy**: Instead of using ACLs for access control, I utilized a bucket policy (`aws_s3_bucket_policy`) to allow public access for my static website files.
    

Here's the final [`main.tf`](http://main.tf) file with the necessary changes:

```plaintext
provider "aws" {
  region = "us-east-1"
}

variable "bucket_name" {
  default = "angular-reactive-formapplica-000099"
}

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

resource "aws_s3_bucket_ownership_controls" "s3_bucket_acl_ownership" {
  bucket = aws_s3_bucket.reactive_form.id

  rule {
    object_ownership = "BucketOwnerEnforced"
  }
}

resource "aws_s3_bucket_public_access_block" "s3_public_block" {
  bucket = aws_s3_bucket.reactive_form.id

  block_public_acls   = true
  block_public_policy = false
  ignore_public_acls  = true
  restrict_public_buckets = false
}

resource "aws_s3_bucket_policy" "allow_public_access" {
  bucket = aws_s3_bucket.reactive_form.id
  policy = data.aws_iam_policy_document.allow_public_access.json
}

data "aws_iam_policy_document" "allow_public_access" {
  statement {
    actions = [
      "s3:GetObject",
      "s3:ListBucket"
    ]
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
  content_type  = lookup(var.mime_types, split(".", each.value)[length(split(".", each.value)) - 1], "application/octet-stream")
}

output "website_endpoint" {
  value = aws_s3_bucket.reactive_form.website_endpoint
}
```

After applying these changes, I successfully eliminated the **AccessControlListNotSupported** error and enabled the deployment of my Angular app to the S3 bucket using Terraform.