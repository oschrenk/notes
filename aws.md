# AWS

Ubuntu Image selector: https://cloud-images.ubuntu.com/locator/ec2/

[AWS Service Health Dashboard](http://status.aws.amazon.com/)

## I am
To later filter for things created by you, this is your owner id. Well….. un fortunately that is some sort of group not yourself. Ugh.

```
aws iam get-user | jq -r .User.Arn | cut -d ':' -f 5
```

## Storage

### Find out all availability zones

```
aws ec2 describe-availability-zones | jq -r .AvailabilityZones[].ZoneName
```

### Find out availability zone of an instance

```
aws ec2 describe-instances --instance-id i-0332d9bea36886653 --output text --query 'Reservations[*].Instances[*].Placement.AvailabilityZone'
```

### Create Volume

* 512 gb
* `gp2` for general purpose ssd

```
aws ec2 create-volume --availability-zone eu-west-1c --size 512 --volume-type gp2 --tag-specifications 'ResourceType=volume,Tags=[{Key=created-by,Value=oschrenk},{Key=purpose,Value=storage}]' | jq -r .VolumeId
vol-05c06d9bdf6c92a8c
```

### List volumes with tag

```
aws ec2 describe-volumes --filters Name=tag-key,Values="created-by" Name=tag-value,Values="oschrenk" | jq -r .Volumes[].VolumeId
```

### Delete volume

```
aws ec2 delete-volume --volume-id vol-07efd77a8f712eff4
```

### Attach volume to instance

```
aws ec2 attach-volume --volume-id vol-05c06d9bdf6c92a8c --instance-id i-0332d9bea36886653 --device /dev/sdf
```

Connect to the instance via ssh

```
ssh ec2-user@...
```

list the available disks

```
lsblk
```

Check if the volume has any data using the following command.

```
sudo file -s /dev/xvdf
```
If the above command output shows `/dev/xvdf: data`, it means your volume is empty.

Format drive

```
sudo mkfs -t ext4 /dev/xvdf
```

Mount

```
sudo mkdir /mnt/storage
sudo mount /dev/xvdf /mnt/storage
sudo chmod u+rw /mnt/storage/
```

## Instances

### Choose the Template

You can use [describe-instances](https://docs.aws.amazon.com/cli/latest/reference/ec2/describe-instances.html)  to list and filter image types

```
# list all ebs-backed, x86_64, hvm linux images from amazon
aws ec2 describe-images --owners self amazon --filters "Name=root-device-type,Values=ebs" "Name=architecture,Values=x86_64" "Name=virtualization-type,Values=hvm" | jq '.Images[] | select(.Platform  != "windows")'
```

Too much useless output. Need to find better things.

```
# latest amazon linux (not sure yet whjat the output means)
aws ec2 describe-images --owners self amazon \
--filters "Name=root-device-type,Values=ebs" "Name=architecture,Values=x86_64" "Name=virtualization-type,Values=hvm" "Name=description,Values=Amazon Linux AMI*" \
--query 'Images[*].[CreationDate,ImageId, Description]' --output text | grep -v test | grep -v Minimal | sort -r | head -4
2018-04-04T15:01:00.000Z        ami-2d386654    Amazon Linux AMI 2017.09.l x86_64 ECS HVM GP2
2018-03-17T03:57:29.000Z        ami-bfb5fec6    Amazon Linux AMI 2017.09.k x86_64 ECS HVM GP2
2018-03-07T08:13:50.000Z        ami-59054720    Amazon Linux AMI 2017.09.1.20180307 x86_64 VPC NAT HVM EBS
2018-03-07T07:04:49.000Z        ami-3bfab942    Amazon Linux AMI 2017.09.1.20180307 x86_64 HVM GP2
```

Also not that useful. Giving up for now as the one being offered in the launcher is `ami-3bfab942` but what qualifies that one?


### List instances

*by key*

```
aws ec2 describe-instances --filters "Name=key-name,Values=oschrenk" --query="Reservations[].Instances[].InstanceId" | jq -r .[]
```

*by tag*

```
aws ec2 describe-instances --filters Name=tag-key,Values="created-by" Name=tag-value,Values="oschrenk" --query="Reservations[].Instances[].InstanceId" | jq -r .[]
```

### Choose the Instance Type

[Amazon EC2 Instance Types Overview](https://aws.amazon.com/ec2/instance-types/)

* *T2* instances provide a baseline level of CPU performance with the ability to burst above the baseline.

| Model | vCPU | CPU Credits / hour | Mem (GiB) |  Storage |
| --- | --- | --- | --- | --- |
| t2.nano | 1 | 3 | 0.5 | EBS-Only |
| t2.micro | 1 | 6 | 1 | EBS-Only |
| t2.small | 1 | 12 | 2 | EBS-Only |
| t2.medium | 2 | 24 | 4 | EBS-Only |
| t2.large | 2 | 36 | 8 | EBS-Only |
| t2.xlarge | 4 | 54 | 16 | EBS-Only |
| t2.2xlarge | 8 | 81 | 32 | EBS-Only |

*M5* instances are the latest generation of General Purpose Instances.

| Model | vCPU | Mem (GiB) | SSD Storage (GB) | Dedicated EBS Bandwidth (Mbps) |
| --- | --- | --- | --- | --- |
| m5.large | 2 | 8 | EBS-only | Up to 2,120  |
| m5.xlarge | 4 | 16 | EBS-only | Up to 2,120  |
| m5.2xlarge | 8 | 32 | EBS-only | Up to 2,120  |
| m5.4xlarge | 16 | 64 | EBS-only | 2,120  |
| m5.12xlarge | 48 | 192 | EBS-only | 5,000  |
| m5.24xlarge  | 96  | 384  | EBS-only  | 10,000  |

*M4* M4 instances provide a balance of compute, memory, and network resources, and it is a good choice for many applications.

| Model | vCPU | Mem (GiB) | SSD Storage (GB) | Dedicated EBS Bandwidth (Mbps) |
| --- | --- | --- | --- | --- |
| m4.large | 2 | 8 | EBS-only | 450  |
| m4.xlarge | 4 | 16 | EBS-only | 750  |
| m4.2xlarge | 8 | 32 | EBS-only | 1,000  |
| m4.4xlarge | 16 | 64 | EBS-only | 2,000  |
| m4.10xlarge | 40 | 160 | EBS-only | 4,000  |
| m4.16xlarge  | 64  | 256  | EBS-only  | 10,000  |

### Create a KeyPair

Go to `https://eu-west-1.console.aws.amazon.com/ec2/v2/home?region=eu-west-1#KeyPairs` and "Create Key Pai". The keyfile is autoamticall being downloaded.

### Choose security group

### Choose subnet

### Create an instance

https://docs.aws.amazon.com/cli/latest/reference/ec2/run-instances.html

```
# I like to only return the instance id
aws ec2 run-instances --image-id ami-3bfab942 --count 1 --instance-type x1e.2xlarge --key-name oschrenk | jq -r .Instances[].InstanceId
i-0332d9bea36886653
```

Medium instance already attached to my "public ssh" security group tagged with a name


```
aws ec2 run-instances --image-id ami-3bfab942 --count 1 --instance-type t2.medium --key-name oschrenk --security-groups public-ssh --tag-specifications 'ResourceType=instance,Tags=[{Key=Name,Value=PrettyName},{Key=owner,Value=oschrenk}]' | jq -r .Instances[].InstanceId

```

### Connect to an instance

Get the public dns name

```
aws ec2 describe-instances \
--output text \
--query "Reservations[].Instances[].PublicDnsName" \
--instance-ids i-0332d9bea36886653
```

Connect via ssh


## Security Groups

### Delete unused security groups

???? There is also [DescribeStaleSecurityGroups - Amazon Elastic Compute Cloud](https://docs.aws.amazon.com/AWSEC2/latest/APIReference/API_DescribeStaleSecurityGroups.html)

First, get a list of all security groups

```
aws ec2 describe-security-groups --query 'SecurityGroups[*].GroupId'  --output text | tr '\t' '\n' > all.txt
```
Then get all security groups tied to an instance, then piped to sort then uniq:

```
aws ec2 describe-instances --query 'Reservations[*].Instances[*].SecurityGroups[*].GroupId' --output text | tr '\t' '\n' | sort | uniq > used.txt
```

Then put it together and compare the 2 lists and see what's not being used from the master list:

```
comm -23  all.txt used.txt
```

### Default Virtual Private Cloud

```
aws ec2 describe-vpcs --output text --filters "Name=is-default,Values=true" --query 'Vpcs[*].VpcId'
vpc-495bb02c
```

### Create Security Group

```
aws ec2 create-security-group --group-name public-ssh --description
 "Group with Public SSH access" --vpc-id vpc-495bb02c | jq -r .GroupId
```

### Add Inbound traffic rule

```
aws ec2 authorize-security-group-ingress --group-id sg-06a3520738dbac886 --protocol tcp --port 22 --cidr 0.0.0.0/0
```

### Modify security group of an existing instance

```
aws ec2 modify-instance-attribute --instance-id i-0332d9bea36886653 --groups sg-06a3520738dbac886
```

### What happens when I reboot an EC2 instance?
As per [AWS Documentation](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/ec2-instance-reboot.html)

> An instance reboot is equivalent to an operating system reboot. In most cases, it takes only a few minutes to reboot your instance. When you reboot an instance, it remains on the same physical host, so your instance keeps its public DNS name (IPv4), private IPv4 address, IPv6 address (if applicable), and any data on its instance store volumes.

> Rebooting an instance doesn't start a new instance billing hour, unlike stopping and restarting your instance.

Further, they recommend:

> We recommend that you use Amazon EC2 to reboot your instance instead of running the operating system reboot command from your instance. If you use Amazon EC2 to reboot your instance, we perform a hard reboot if the instance does not cleanly shut down within four minutes.

```
aws ec2 reboot-instances --instance-ids <instance-id>
```

### What happens if I start/stop an EC2 instance?

[Stop and Start Your Instance - Amazon Elastic Compute Cloud](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/Stop_Start.html)

> You can stop and restart your instance if it has an Amazon EBS volume as its root device. The instance retains its instance ID […]

To check that

```
aws ec2 describe-instances --instance-ids <instance-id> | jq .Reservations[].Instances[0].RootDeviceType
```

> When you stop an instance, we shut it down.

> While the instance is stopped, you can treat its root volume like any other volume, and modify it (for example, repair file system problems or update software). You just detach the volume from the stopped instance, attach it to a running instance, make your changes, detach it from the running instance, and then reattach it to the stopped instance. Make sure that you reattach it using the storage device name that's specified as the root device in the block device mapping for the instance.

[Instance Lifecycle - Amazon Elastic Compute Cloud](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/ec2-instance-lifecycle.html)

> If your instance fails a status check or is not running your applications as expected, and if the root volume of your instance is an Amazon EBS volume, you can stop and start your instance to try to fix the problem.

> When you stop your instance, it enters the `stopping` state, and then the `stopped` state.

> When you start your instance, it enters the `pending` state, and in most cases, we move the instance to a new host computer. (Your instance may stay on the same host computer if there are no problems with the host computer.) When you stop and start your instance, you lose any data on the instance store volumes on the previous host computer.


### How to troubleshoot an unreachable EC2 instance?

[Troubleshoot an Unresponsive EC2 Instance](https://aws.amazon.com/premiumsupport/knowledge-center/ec2-unresponsive-troubleshoot/)


#### Take screenshot

```
aws ec2 get-console-screenshot --instance-id <instance-id> | jq -r .ImageData | base64 --decode > screenshot.jpg
```

## Storage
* `Root device type`: `ebs`
* `Root device`: `/dev/xvda`
* `Block devices`: `/dev/xvda`

## Appendix
### Definitions

*AMI*
See Amazon Machine Instance

*Amazon Machine Instance*
[Instances and AMIs - Amazon Elastic Compute Cloud](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/ec2-instances-and-amis.html#instances)
> An Amazon Machine Image (AMI) is a template that contains a software configuration (for example, an operating system, an application server, and applications). From an AMI, you launch an instance, which is a copy of the AMI running as a virtual server in the cloud. You can launch multiple instances of an AMI, as shown in the following figure.

*EBS*
See Elastic Block Storage

*Elastic Block Storage*

*Instance*

*Instance store volume*

> The local instance store volumes that are available to the instance. The data in an instance store is not permanent - it persists only during the lifetime of the instance.

*Root device volume*
[Amazon EC2 Root Device Volume - Amazon Elastic Compute Cloud](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/RootDeviceStorage.html)

> When you launch an instance, the /root device volume/ contains the image used to boot the instance.
> […]
> We recommend that you use AMIs backed by Amazon EBS, because they launch faster and use persistent storage.

*VPC*
See Virtual Private Cloud

## IAM

I want a machine instance to have selective access to a single docker image hosted by ECS.

I need to to know the region, the account, the repository name.

* where do I find the account number
* repository is <group>/<project>
