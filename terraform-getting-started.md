# Getting Started with Terraform

HashiCorp Terraform is an open-source infrastructure as code (IaC) software tool. IaC lets you manage and provision infrastructure through code rather than through a graphical user interface. This enables you to eliminate manual processes, iterate faster, and maintain consistency as your infrastructure presence grows. 

## Learning Objectives
In this tutorial, you will learn how to: 

- Install Terraform 
- Write a Terraform configuration file to provision a Docker container 
- Destroy resources using Terraform 

## Prerequisites

- [Docker Desktop installed](https://www.docker.com/products/docker-desktop/)
- Command line experience

## Install Terraform

To use Terraform, you need to install it. Visit [Terraform.io](https://www.terraform.io/downloads.html), find the appropriate package for your system and download the zip archive. After downloading Terraform, unzip the package.



Finally, make sure the `terraform` binary is available on your `PATH`. This process differs depending on your operating system: 

- [Mac or Linux](https://stackoverflow.com/questions/14637979/how-to-permanently-set-path-on-linux)
- [Windows](https://stackoverflow.com/questions/1618280/where-can-i-set-path-to-make-exe-on-windows)

## Verify the Installation
To verify the installation worked, open a new terminal window and list the Terraform version. 
```shell
$ terraform -version
```

## Provision Infrastructure
Now that you've installed Terraform, you can now provision infrastructure. In this tutorial, you will use Terraform to provision an NGINX server using [Docker Desktop](https://www.docker.com/products/docker-desktop/). 

After you install Docker Desktop, start the Docker app. This command opens Docker Desktop.  
```shell
$ open -a Docker
```

You can now begin writing Terraform configuration code. To start, create a new directory on your local machine called `terraform-demo`. 

```shell
$ mkdir terraform-demo
```

Then, navigate into it. 

```shell
$ cd terraform-demo
```

Next, create a file for your Terraform configuration code.

```shell
$ touch main.tf
```

Paste the following code into the `main.tf` file.

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

Initialize Terraform with the `init` command. This command initializes the directory and downloads the necessary provider plugins so that Terraform can interact with Docker. 

```shell
$ terraform init
```

Check for any errors. If the `init` command ran successfully, provision the resource with the `apply` command. Terraform will ask to confirm this action. Type `yes` and press `ENTER` to confirm. 

```shell
$ terraform apply
```

The command takes a few minutes to run. After the command completes, it displays a message indicating the resource was created. 

## Validate Results 

To verify that your NGINX server was deployed, navigate to the Docker Desktop app. Click on `Containers/Apps` on the left side panel. Verify that your container is listed. Alternatively, run `docker ps` in the terminal to verify your container is running. 

Finally, open a web browser and navigate to [localhost:80](http://localhost:80). You should see the NGINX landing page.

![NGINX Landing Page in browser at localhost:80](https://lanalayt-bucket-hashi.s3.us-west-2.amazonaws.com/nginxlandingpage.png)

## Clean Up  

Once you verified the existence of your container, destroy your infrastructure. Terraform will ask to confirm this action. Type `yes` and press `ENTER` to confirm. 

```shell
$ terraform destroy
```
After the command completes, it displays a message indicating that Terraform has destroyed all resources. 

## Next steps

You have now safely and efficiently provisioned and destroyed an NGINX server using Docker and Terraform. Next, learn the basics of Terraform with the platform of your choice. 
- [AWS](https://learn.hashicorp.com/collections/terraform/aws-get-started)
- [Azure](https://learn.hashicorp.com/collections/terraform/azure-get-started)
- [Google Cloud](https://learn.hashicorp.com/collections/terraform/gcp-get-started)
- [OCI](https://learn.hashicorp.com/collections/terraform/oci-get-started)
- [Terraform Cloud](https://learn.hashicorp.com/collections/terraform/cloud-get-started)
- [Docker](https://learn.hashicorp.com/collections/terraform/docker-get-started)
