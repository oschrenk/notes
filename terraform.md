# Terraform

```
brew install terraform
```

## Requirements

AWS account with access key

## Basics

> Terraform code is written in a language called HCL in files with the extension “.tf”. It is a declarative language, so your goal is to describe the infrastructure you want, and Terraform will figure out how to create it.

> Terraform can create infrastructure across a wide variety of platforms, or what it calls providers, including AWS, Azure, Google Cloud, DigitalOcean, and many others.

## Deploy a single server

Create a file called `main.tf` with this content:

```
provider "aws" {
  region = "eu-west-1"
}
```

This tells Terraform that you are going to be using the AWS provider and that you wish to deploy your infrastructure in the “eu-west-1” region

Add

```
resource "aws_instance" "example" {
  ami = "ami-3bfab942"
  instance_type = "t2.micro"
}
```

Each resource specifies a type (in this case, `aws_instance`), a name (in this case `example`) to use as an identifier within the Terraform code, and a set of configuration parameters specific to the resource. The [aws_instance resource documentation](https://www.terraform.io/docs/providers/aws/r/instance.html) lists all the parameters it supports. Initially, you only need to set the following ones:

* `ami`. The Amazon Machine Image to run on the EC2 Instance. The example above sets this parameter to the ID of an Ubuntu 14.04 AMI
* `instance_type`. Type of EC2 Instance to run. Each EC2 Instance Type has different amount CPU, memory, disk space, and networking specs. The example above uses “t2.micro”, which has 1 virtual CPU, 1GB of memory.

Run

1. `terraform init` to install plugins
2. `terraform plan` lets you see what Terraform will do before actually doing it. This is a great way to sanity check your changes. The output is a little like the output of the diff command.
3. `terraform apply` to actually apply the changes

You can check on the [ec2 console] that changes actually were applied

But let's step up our game. Let's name the machine.

Change the resource


## Workflow

1. load desired configuration
2. load the .tfstate file
3. calculate diff between current and desired state
4. use API calls to apply changes to match desired state
5. update state file

##

one recommendations was to have separate accounts for each environment

always use remote state. it's an artifact state doesn;t belong thre

## Glossary

plan is the diff between what exists in your infrastructure and what you want to apply

workspace
module:
a collection of resources
Blackbox of infrastructure with inout and output variables eg. Java app. Inout: Path to app, output path to loadbalancer.le
variable: dynaimic configured inouts
  can be typed!!!
  named
  description
  defaults
resource configuration for a specific entity (instance, load balancer, ...)
  they take inouts and can produce outputs. they may interpolate vsriables

  ```
  resource "<type>" "<id>" {
    ... = "..."  // inputs

  }
  ```
output = take created outoputs and use them somewhere
data =
data elements allow you to pull information form your provider and pull
values

hcl hashicorp configuration  languagek
.tf
.tfvars
.tfstate -
  state file Terraform oeprates from this file as the single source of truth

## Resources

https://blog.gruntwork.io/an-introduction-to-terraform-f17df9c6d180
