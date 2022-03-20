# Getting Started with Terraform

HashiCorp Terraform is an open-source infrastructure as code (IaC) software tool. IaC lets you manage and provision infrastructure through code rather than through a graphical user interface. This allows you to eliminate manual processes, iterate faster, and maintain consistency as your infrastructure presence grows. 

# Installing Terraform
To use Terraform, you will need to install it. You can install Terraform by visiting [Terraform.io](https://www.terraform.io/downloads.html). Find the appropriate package for your system and download it as a zip archive. After downloading Terraform, unzip the package. Finally, make sure the `terraform` binary is available on your `PATH`. This [HashiCorp tutorial](https://learn.hashicorp.com/tutorials/terraform/install-cli?in=terraform/aws-get-started) contains instructions for adding binaries to your PATH based on your operating system. 

#Verify the Installation
To verify the installation worked, open a new terminal window and list the Terraform version. 
```shell
$ terraform -version
```

With Terraform installed, let's dive right into it and start creating some infrastructure.

Most guys find it easiest to create a new directory on there local machine and create Terraform configuration code inside it.

```shell
$ mkdir terraform-demo
$ cd terraform-demo
```

Next, create a file for your Terraform configuration code.

```shell
$ touch main.tf
```

Paste the following lines into the file.

```hcl
terraform {
  required_providers {
    docker = {
      source = "kreuzwerker/docker"
    }
  }
}
provider "docker" {
    host = "unix:///var/run/docker.sock"
}
resource "docker_container" "nginx" {
  image = docker_image.nginx.latest
  name  = "training"
  ports {
    internal = 80
    external = 80
  }
}
resource "docker_image" "nginx" {
  name = "nginx:latest"
}
```

Initialize Terraform with the `init` command. The AWS provider will be installed. 

```shell
$ terraform init
```

You shoud check for any errors. If it ran successfully, provision the resource with the `apply` command.

```shell
$ terraform apply
```

The command will take up to a few minutes to run and will display a message indicating that the resource was created.

Finally, destroy the infrastructure.

```shell
$ terraform destroy
```

Look for a message are the bottom of the output asking for confirmation. Type `yes` and hit ENTER. Terraform will destroy the resources it had created earlier.
