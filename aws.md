# Amazon Web Services #

- offers a 1 year free micro tier

## Registration ##

During the registration process they need your Credit Card information and a phone number on which an automatic call will be placed

## Configuring an instance ##

The process can be quite complex, with all the options the offer. The default seem to be quite reasonable though. I feel that they are two ideas or concepts that are quite important to understand, besides choosing the image, cpu and storage solution.

### Security Groups ###

Security groups define access incoming and outgoing traffic rules to this machine. They can be defined based on ip adresses on port numbers. For a machine that should act as a web server you should define a security group that allows public access to port `80` (`HTTP`) and `443` (`HTTPS`).

### Identity file ###

At the end of the wizard asks you to choose an existing or create a new identity file to access this machine. Download the file to your machine and store it in secure (and backuped) place. I myself put it in `~/.ssh` and rely on a encrypted local and encrypted remote backup.

I highly recommend to put the host information and the associated key inside your ssh configuration to make access and life easier.

	vi ~/.ssh/config

and put

	Host aws
	  HostName ec2-xx-xxx-xxx-xxx.us-west-2.compute.amazonaws.com
	  IdentityFile ~/.ssh/aws.pem
	  User ubuntu

This allows you to connect to your machine by just using

	ssh aws

instead of `ssh -i ~/ssh/aws.pem ubuntu@ec2-xx-xxx-xxx-xxx.us-west-2.compute.amazonaws.com` which may be hard to remember and definitely slow to type.
