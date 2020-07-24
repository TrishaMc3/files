# Get Started with Terraform

Define and provision infrastructure as code (IaC) with Terraform, the most popular IaC language. In this guide, you learn to install, initialize, provision, and maximize the performance of Terraform on your local system. 
## Prerequisites
* Platform, machine, or environment for code and app development 
* Applicable package for your OS from [Download Terraform](https://www.terraform.io/downloads.html) 

After you download Terraform, you can create its installation infrastructure.
## Create a directory
We recommend that you create a new directory on your local machine. 

```shell
$ mkdir terraform-demo
$ cd terraform-demo
```
## Create a file
Create a file inside the directory for your Terraform configuration code.

```shell
$ touch main.tf
```
## Add code to the file
Copy this code and paste it into the file.

```hcl
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
## Initialize Terraform 
To initialize Terraform, run the `init` command. The Docker provider installs. 

```shell
$ terraform init
```
## Test your installation
Check your installation for errors. 
## Provision your resource
If your code ran successfully, provision the resource with the `apply` command.

```shell
$ terraform apply
```

The command might take a few minutes to run. When the resource is created, a message displays.
## Destroy the infrastructure
Finally, destroy the Terraform installation infrastructure.

```shell
$ terraform destroy
```

When you see a message that asks for confirmation, type `yes` and select ENTER. Terraform destroys the resources it created earlier.

## Next steps
In this get-started guide, you learned to install and provision Terraform. To maximize performance, you removed the installation architecture. Now you can define and provision IaC with the Terraform language on your local OS.
Learn more about [Terraform](https://www.terraform.io).

