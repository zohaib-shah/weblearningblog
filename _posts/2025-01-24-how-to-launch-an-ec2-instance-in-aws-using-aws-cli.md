---
layout: post
title: "How to Launch an EC2 Instance in AWS Using AWS CLI"
date: 2025-01-24
categories: [AWS]
tags: [aws-cli, ec2, nginx, ubuntu, cloud]
author: Zohaib

---

# How to Launch an EC2 Instance in AWS Using AWS CLI
In this comprehensive guide, we'll explore how to launch an EC2 instance using the AWS CLI. By the end of this tutorial, you'll have an Ubuntu-based EC2 instance running with Nginx installed via user data.

## Overview

The EC2 instance will be configured to serve as a web server with Nginx pre-installed. We'll use Ubuntu 22.04 as the base image for our EC2 instance. AWS provides Amazon Machine Images (AMI) for various operating systems, making it easy to get started.

Security groups act as firewalls for EC2 instances. Each EC2 instance must have one or more associated security groups that define both inbound and outbound traffic rules.

To access the EC2 instance via SSH, we need to attach an SSH key pair. This key pair can either be newly created for the EC2 instance or an existing one available in the specific AWS region.

### Prerequisites

For this tutorial, we will use the following AWS CLI version:

```bash
aws-cli/2.22.23
```

## Finding AWS AMI for Ubuntu 22.04

To launch an EC2 instance based on Ubuntu, we first need to find the appropriate AMI.

### What is an AMI?

An EC2 AMI (Amazon Machine Image) is a pre-configured virtual machine image used to launch an Amazon Elastic Compute Cloud (EC2) instance. It includes the operating system, application server, applications, and necessary configurations to run a specific environment. When launching an EC2 instance, the selected AMI serves as the template for creating the instance.

### Finding Ubuntu AMIs

To find the available Ubuntu AMIs on AWS, run the following command:

```bash
aws ec2 describe-images \
  --owners 099720109477 \
  --filters "Name=name,Values=ubuntu/images/hvm-ssd/ubuntu-*-22.04-amd64-server-*" \
            "Name=state,Values=available" \
  --query "Images[*].{ID:ImageId,Name:Name}" \
  --region us-east-1 \
  --output table
```

Running this command will display a table of available AMIs in the following format:

```
-----------------------------------------------------------------------------------------------
|                                       DescribeImages                                        |
+------------------------+--------------------------------------------------------------------+
|           ID           |                               Name                                 |
+------------------------+--------------------------------------------------------------------+
|  ami-012485deee5681dc0 |  ubuntu/images/hvm-ssd/ubuntu-jammy-22.04-amd64-server-20240514    |
|  ami-04a98573e58903ee0 |  ubuntu/images/hvm-ssd/ubuntu-jammy-22.04-amd64-server-20240913    |
|  ami-06a1f46caddb5669e |  ubuntu/images/hvm-ssd/ubuntu-jammy-22.04-amd64-server-20230608    |
|  ami-0337c8d6fb02b2dcf |  ubuntu/images/hvm-ssd/ubuntu-jammy-22.04-amd64-server-20240119.1  |
|  ami-03a4363a7d864a093 |  ubuntu/images/hvm-ssd/ubuntu-jammy-22.04-amd64-server-20230711    |
+------------------------+--------------------------------------------------------------------+
```

## Working with SSH Key Pairs

When launching an EC2 instance, it is essential to set a key pair to enable SSH access.

### List Existing Key Pairs

To list all the available key pairs in a specific AWS region, use the following command:

```bash
aws ec2 describe-key-pairs --region us-east-1
```

The output of the above command will look something like this if you have previously created any key pairs:

```json
{
    "KeyPairs": [
        {
            "KeyPairId": "key-08255e3779#######",
            "KeyType": "rsa",
            "Tags": [],
            "CreateTime": "2023-04-28T20:56:41.342000+00:00",
            "KeyName": "name-of-the-keypair",
            "KeyFingerprint": "##:##:##:##:##:##:##:b4:2f:55:91:a4:32:a1:cf:f4:ca:1a:cc:f8"
        }
    ]
}
```

If you already have key pairs and want to reuse one for accessing the EC2 instance, you can simply use the `KeyName` from the response.

### Create a New Key Pair

If you don't have any existing key pairs or prefer to create a dedicated one specifically for EC2 access, you can generate a new key pair using the following command:

```bash
aws ec2 create-key-pair \
  --key-name ec2-access-key \
  --query "KeyMaterial" \
  --region us-east-1 \
  --output text \
> ec2-access-key.pem
```

This command creates a new EC2 key pair named `ec2-access-key` in the `us-east-1` region. The `--query "KeyMaterial"` option ensures that only the private key (the key material) is returned in the output, which is essential for SSH access to EC2 instances. The `--output text` flag formats the output as plain text, which is the private key content. The redirection operator (`>`) then saves the private key to a file named `ec2-access-key.pem` in the current directory. This `.pem` file can be used for authenticating SSH connections to EC2 instances that are associated with the newly created key pair.

## Create Security Group

AWS Security Groups (SGs) act as virtual firewalls for EC2 instances by controlling incoming and outgoing traffic. They operate in a stateful manner—when inbound traffic is allowed, the corresponding response traffic is automatically permitted, regardless of outbound rules. You can attach Security Groups to various AWS resources, including EC2 instances, RDS instances, and load balancers. Each instance can have multiple security groups attached to it.

To create a security group using the AWS CLI, we will begin by defining the security group and then proceed to configure its inbound and outbound rules.

```bash
aws ec2 create-security-group \
    --group-name nginx-web-server-sg \
    --description "Security group for NGINX web server" \
    --vpc-id <your-vpc-id> \
    --region us-east-1
```

The response of the above command will be as follows:

```json
{
    "GroupId": "sg-0050df0314cc0a###",
    "SecurityGroupArn": "arn:aws:ec2:us-east-1:45156######:security-group/sg-0050df0314cc0a###"
}
```

### Add Inbound Rules

To allow HTTP (port 80) traffic, run the following command:

```bash
aws ec2 authorize-security-group-ingress \
    --group-name nginx-web-server-sg \
    --protocol tcp \
    --port 80 \
    --cidr 0.0.0.0/0 \
    --region us-east-1
```

This should result in something like below:

```json
{
    "Return": true,
    "SecurityGroupRules": [
        {
            "SecurityGroupRuleId": "sgr-0d401d350d1b7####",
            "GroupId": "sg-0050df0314######",
            "GroupOwnerId": "451563#####",
            "IsEgress": false,
            "IpProtocol": "tcp",
            "FromPort": 80,
            "ToPort": 80,
            "CidrIpv4": "0.0.0.0/0",
            "SecurityGroupRuleArn": "arn:aws:ec2:us-east-1:45156373####:security-group-rule/sgr-0d401d350d1######"
        }
    ]
}
```

Let's add another inbound rule to allow SSH traffic on port 22. For now, we are allowing SSH traffic from all IPs, but we also have the option to set a CIDR block to restrict access to a specific IP address or range.

```bash
aws ec2 authorize-security-group-ingress \
    --group-name nginx-web-server-sg \
    --protocol tcp \
    --port 22 \
    --cidr 0.0.0.0/0 \
    --region us-east-1
```

> **Note:** While launching an EC2 instance as an NGINX server, an outbound rule is not required. However, if you want to restrict outbound traffic, you can use the `authorize-security-group-egress` command.

## Create User Data Script

**User Data** in EC2 is a set of commands or a script that runs when launching an Amazon EC2 instance. It automates initialization tasks like installing software, configuring applications, and performing setup steps.

Let's create a bash script that we'll use when launching the instance.

In your current directory, create a file named `user-data.sh` with the following content:

```bash
#!/bin/bash
# Update package list and install NGINX
apt-get update -y
apt-get install nginx -y

# Create a simple "Hello World" page
echo "Hello, World from NGINX on EC2!" > /var/www/html/index.html

# Start and enable NGINX
systemctl start nginx
systemctl enable nginx
```

## Launching the EC2 Instance

Now that we have all the prerequisites in place, let's launch the EC2 instance:

```bash
aws ec2 run-instances \
    --image-id ami-000b79ebd8ab##### \
    --count 1 \
    --instance-type t2.micro \
    --key-name ec2-access-key \
    --security-group-ids sg-0050df0314cc##### \
    --user-data file://user-data.sh \
    --region us-east-1
```

If the command is successful, you'll receive instance details in the response. You can then go to the EC2 instances page in your AWS Management Console to see the running instance.

### Verify the Installation

To verify the installation, copy your EC2 instance's public IP address and enter `http://<your-instance-public-ip>` in your browser. You should see the welcome message from the Nginx server.

## Managing EC2 Instances

### List Running Instances

To list all running instances, use this command:

```bash
aws ec2 describe-instances --filters "Name=instance-state-name,Values=running" --region us-east-1
```

Since this command returns extensive details that can be hard to parse, you can filter the output to show only essential information using the `--query` parameter:

```bash
aws ec2 describe-instances --query "Reservations[].Instances[].{ID:InstanceId,State:State.Name}" --output table --region us-east-1
```

### Stop an Instance

After obtaining the instance ID, you can perform operations like stopping the instance:

```bash
aws ec2 stop-instances --region us-east-1 --instance-ids i-0c98b2bd9dfd#####
```

This command will generate a response like this:

```json
{
    "StoppingInstances": [
        {
            "InstanceId": "i-0c98b2bd9#######",
            "CurrentState": {
                "Code": 64,
                "Name": "stopping"
            },
            "PreviousState": {
                "Code": 64,
                "Name": "stopping"
            }
        }
    ]
}
```

The instance state will first change to "stopping" and then to "stopped" after a few seconds.

### Terminate an Instance

To completely terminate the instance, use the following command:

```bash
aws ec2 terminate-instances --region us-east-1 --instance-ids i-0c98b2bd9dfd######
```

## Conclusion

In this guide, we've covered the complete process of launching and managing an EC2 instance using the AWS CLI. We've learned how to:

- Find suitable AMIs for Ubuntu 22.04
- Create and manage SSH key pairs
- Configure security groups with appropriate inbound rules
- Create user data scripts for instance initialization
- Launch an EC2 instance with all the necessary configurations
- Manage instances (list, stop, and terminate)

Using the AWS CLI provides a powerful way to automate your infrastructure and integrate it into your DevOps workflows. You can use these commands in scripts, CI/CD pipelines, or infrastructure-as-code tools to manage your AWS resources programmatically.
