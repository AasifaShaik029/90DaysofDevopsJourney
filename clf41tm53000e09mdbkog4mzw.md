---
title: "Terraform 01: An Introduction to the Terraform Series"
datePublished: Sat Mar 11 2023 14:15:07 GMT+0000 (Coordinated Universal Time)
cuid: clf41tm53000e09mdbkog4mzw
slug: terraform-01-an-introduction-to-the-terraform-series
cover: https://cdn.hashnode.com/res/hashnode/image/upload/v1678544016178/6ba3f720-9b1a-4e4c-b4b1-dd19ba693077.png
tags: docker, devops, terraform

---

### Terraform

Terraform is an open-source infrastructure as code (IaC) tool developed by HashiCorp. It allows you to define and manage your cloud infrastructure in a declarative manner, using code to define and configure the resources that make up your infrastructure.

### Why Terraform?

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1678453441539/0aa468a5-26d4-4bda-b413-5ec87028d984.png align="center")

In order to deploy an application, we need infrastructure like servers, load balancers, databases, etc. If we use all of these manually, then as humans we may mistakes like how many instances we used, what type of software used, etc. So it will be difficult to restore that mistake. Also, if an employee resigns, we don't know what he has done for the infrastructure. Few of the challenges were mentioned above. Here comes Terraform which is an Infrastructure as Code tool. Everything will be managed using code.

### Installation

Follow this URL for installation

[Install Terraform | Terraform | HashiCorp Developer](https://developer.hashicorp.com/terraform/tutorials/aws-get-started/install-cli)

```plaintext
sudo apt-get update && sudo apt-get install -y gnupg software-properties-common
```

```plaintext
wget -O- https://apt.releases.hashicorp.com/gpg | \
    gpg --dearmor | \
    sudo tee /usr/share/keyrings/hashicorp-archive-keyring.gpg
```

```plaintext
gpg --no-default-keyring \
    --keyring /usr/share/keyrings/hashicorp-archive-keyring.gpg \
    --fingerprint
```

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1678463586173/f0f534c0-e7e9-4963-836a-06a84c366a8f.png align="center")

```plaintext
echo "deb [signed-by=/usr/share/keyrings/hashicorp-archive-keyring.gpg] \
    https://apt.releases.hashicorp.com $(lsb_release -cs) main" | \
    sudo tee /etc/apt/sources.list.d/hashicorp.list
```

```plaintext
sudo apt update
```

```plaintext
sudo apt-get install terraform

terraform --version
```

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1678463757262/bef88a01-3d79-435a-8d14-7b3c89f96796.png align="center")

Terraform is now installed 😀

---

### Hands-On

Let's create a directory for terraform practice.

🟪 AIM: Want to create a file. Generally, we will use vim devopsfile.txt and then enter text as below.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1678511297884/ef8db056-ee01-4796-ba3e-120c8ffeb8a1.png align="center")

Let's tell the terraform to create the same file without manual effort. 😃

Lets keep our automation work in one directory.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1678513031086/1ddf82d7-6762-4876-a664-b1ebe6891710.png align="center")

We will be writing our IAC in one file with extension "tf". In this file we use HCL (Hashocorp language) which is a terraform language.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1678513618826/d448c37d-f21e-49d5-878a-e77bfbcc32f6.png align="center")

There will be a block with { } braces. Inside block there will be parameters.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1678513852016/c549f184-99b4-4cbc-aa1e-d96e9d9245ee.png align="center")

resource is a block name that expects two labels. 1. Type of the resource and 2. Name of the resource.

Inside the block we can give filename and content.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1678519224539/fb8040f6-a0b5-4214-9553-20ee130dade3.png align="center")

```plaintext
resource "local_file" "devops" {
        filename = "/home/ubuntu/terraform-projects/terraform-local/automatedfile.txt"
        content = " Thsi is my devops file created by terraform"

}
```

I am creating a resource called devops for local file type. I gave name for name to my file with the path and at the last with file name called automatedfile.txt. Also added content as above.

### Running the Terraform Code

1. We need to initiate our resources. Everytime when we add a resource in future that file should be initiated.
    
    Initiate with
    

```plaintext
terraform init
```

1. Validate terraform using
    

```plaintext
terraform validate
```

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1678519691735/520c60fc-5573-4381-bb5f-59d4fce9bedb.png align="center")

1. To view the plan of what terraform is going to perform use
    

```plaintext
terraform plan
```

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1678519769163/7661c992-7f2e-49df-aba9-a685221ce743.png align="center")

1. Apply the execution plan using
    

```plaintext
terraform apply
```

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1678519898489/dc76d8fe-6f56-4123-a8c0-4bfeadd3d05d.png align="center")

Give yes to tell the terraform to perform the actions. Now our file is applied. Lets check if it created file automatically with the above content added.

Go to the path as mentioned in the file.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1678520082658/02f6f791-01b1-448a-b78e-f03041a60433.png align="center")

This is how terraform automates the things using code.😉

Note: Whatever terraform does, it will maintain one log file called **terraform.tfstate**

---

### Multiple resources in one file

We can have multiple resources in one file. Let's see

Terraform will create a random string with 20 characters, where it allows special characters, except !#$. Then it will give the output where it displays all the characters terraform created.

```plaintext

resource "random_string" "rand-str" {
length = 20
special = true
override_special ="!#$"
}



output "rand-str" {
value = random_string.rand-str[*].result
}
```

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1678521279843/83a01c12-5f97-4b6a-90a1-50bba9ad54e3.png align="center")

init , validate and apply the new resource.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1678521562596/066e62d4-8689-4b30-b54b-eda0848361b2.png align="center")

our output is generated

---

### Terraform with Docker

Let's automate docker using terraform. We can install required providers under terraform block.

```plaintext
 terraform {
        required_providers {
        docker = {
        source = "kreuzwerker/docker"
        version = "~> 2.21.0"
}
}
}
provider "docker" {}

resource "docker_image" "nginx" {
name = "nginx:latest"
keep_locally = false

}
resource "docker_container" "nginx_ctr" {
image = docker_image.nginx.latest
name = "nginx-tf"
ports {
internal = 80
external = 80
}
}
```

Lets go through this code,

```plaintext
required_providers {
  docker = {
    source = "kreuzwerker/docker"
    version = "~> 2.21.0"
  }
}
```

This section specifies that the docker provider is required for this configuration to work. It also specifies the version of the provider to use, which is 2.21.0 or newer, and the source of the provider, which is kreuzwerker/docker.

```plaintext
provider "docker" {}
```

This section specifies the docker provider to use for the resources defined in this configuration file. It doesn't have any specific configuration options, so it's just an empty block.

```plaintext
resource "docker_image" "nginx" {
  name = "nginx:latest"
  keep_locally = false
}
```

This section defines a Docker image resource with the name nginx. It specifies that the image to use is nginx:latest and sets keep\_locally to false, which means that the image won't be stored on the local machine after it's used.

```plaintext
resource "docker_container" "nginx_ctr" {
  image = docker_image.nginx.latest
  name = "nginx-tf"
  ports {
    internal = 80
    external = 80
  }
}
```

This section defines a Docker container resource with the name nginx\_ctr. It specifies that the container should use the nginx image that was defined earlier, and gives the container the name nginx-tf. It also sets up port forwarding between the container and the host machine by mapping port 80 on the container to port 80 on the host machine.

When we apply the code, using **terraform apply** we get below error. Because we should install docker in our system.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1678541965135/2aa7a773-cbfd-4c9a-9199-40bac37c609c.png align="center")

Note : You cannot add Docker installation in the above code. Terraform is a tool for creating and managing infrastructure, and it assumes that the infrastructure it manages is already in place, including any required software or dependencies. In the case of using the Docker provider, Terraform assumes that Docker is already installed on the machine or machines where the Docker resources will be created.

If you need to install Docker as part of your infrastructure provisioning process, you would need to use a different tool or script to perform the installation, such as a shell script or configuration management tool like Ansible or Chef. Once Docker is installed, you can then use Terraform to create and manage Docker resources, as shown in the example code you provided.

---

\--- 😊Thats all for introduction to terraform, Let's meet in the next concepts on terraform. Thank you for your time 😊 --