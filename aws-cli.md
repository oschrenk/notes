# aws cli

```
brew install awscli
```

## Confiugration

```
mkdir -p ~/.aws
touch ~/.aws/credentials
touch ~/.aws/config
```

*Credentials*

If you don't have credentials, go to [https://console.aws.amazon.com/iam/home#/users], search for your account, click on Security credentials, and choose "Create access key"

```
[default]
aws_access_key_id = your_access_key
aws_secret_access_key = your_secret_key
```

Make sure you have a valid `~/.aws/config` file, specifically with a valid region setting

```
[default]
region=eu-west-1
```

## Common Tasks

List running instances

```
aws ec2 describe-instances --query 'Reservations[*].Instances[*].[InstanceId]' --filters Name=instance-state-name,Values=running --output text
```

