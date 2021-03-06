# Terraform

Terraform can create infrastructure across a wide variety of platforms, or what it calls providers, including AWS, Azure, Google Cloud, DigitalOcean, and many others.

```
brew install terraform

# or via tfenv
brew install tfenv
tfenv install ...
```

## Syntax

> Terraform code is written in a language called HCL in files with the extension “.tf”. It is a declarative language, so your goal is to describe the infrastructure you want, and Terraform will figure out how to create it.


## Usage

### Example: Deploy a server

Create a file called `main.tf` with this content:

```
provider "aws" {
  region = "eu-west-1"
}
```

This tells Terraform that you are going to be using the AWS provider and that you wish to deploy your infrastructure in the `eu-west-1` region

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

## Good practices

* always use remote state.

## Glossary

* `plan`. is the diff between what exists in your infrastructure and what you want to apply
* `workspace`
* `module`: A collection of resources. Blackbox of infrastructure with input and output variables
* `variable`: Dynamic configured input. Can be typed. Supports default values.

## FAQ

### Error: Error locking state: Error acquiring the state lock: ConditionalCheckFailedException: The conditional request failed

It can happen for various reasons that a process could not finish and kept the lock file eg. having bad wifi connection

```
Error: Error locking state: Error acquiring the state lock: ConditionalCheckFailedException: The conditional request failed
        status code: 400, request id: F9TE3PDI83CDTAVNVP54Q35BMVVV4KQNSO5AEMVJF66Q9ASUAAJG
Lock Info:
  ID:        302f9199-fb46-62f2-59b8-df17f579e0d6
  Path:      <<bucket>>/terraform/terraform.tfstate
  Operation: OperationTypeApply
  Who:       schrenko@ELSAMSM-161721.science.regn.net
  Version:   0.11.14
  Created:   2019-06-18 13:21:11.110219 +0000 UTC
  Info:


Terraform acquires a state lock to protect the state from being written
by multiple users at the same time. Please resolve the issue above and try
again. For most commands, you can disable locking with the "-lock=false"
flag, but this is not recommended.
```

Run `terraform force-unlock <lock-id>` and confirm with `yes` if you are sure

## Resources

https://blog.gruntwork.io/an-introduction-to-terraform-f17df9c6d180
