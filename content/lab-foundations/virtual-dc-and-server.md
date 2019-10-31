---
title: "Creating a Virtual DC and placing a Cloud Server"
date: 2019-03-29T17:58:18+03:00
weight: 2
---


## Objectives
Let’s look into creating virtual data centers in the cloud. This exercise guides you through the steps to create a nondefault VPC with a public subnet, and to launch an instance into your subnet.

This virtual network closely resembles a traditional network that you'd operate in your own data center, with the benefits of using the scalable infrastructure of AWS. 

## Services Used in this Lab:
- **Virtual private cloud (VPC)* is a virtual network dedicated to your AWS account. It is logically isolated from other virtual networks in the AWS Cloud. You can launch your AWS resources, such as Amazon EC2 instances, into your VPC. You can specify an IP address range for the VPC, add subnets, associate security groups, and configure route tables. 

## DC Concepts

- *Availability Zones:* Amazon cloud computing resources are housed in highly available data center facilities in different areas of the world. Each Region contains multiple distinct locations called 'Availability Zones,' or AZs. You have the ability to choose which Region to host your Amazon RDS activity in.

## VPC Concepts

- Accessing the Internet (Both directions): You control how the instances that you launch into a VPC access resources outside the VPC. Instances in the cloud can communicate with the internet through the internet gateway. An internet gateway enables your instances to connect to the internet through the Amazon EC2 network edge.
- Accessing the Internet (Outbound only): Alternatively, to allow an instance in your VPC to initiate outbound connections to the internet but prevent unsolicited inbound connections from the internet, you can use a network address translation (NAT) device for IPv4 traffic.
- Subnet: A subnet is a range of IP addresses in your VPC. You can launch AWS resources into a specified subnet. Use a *public subnet* for resources that must be connected to the internet, and a *private subnet* for resources that won't be connected to the internet. 
- To protect the AWS resources in each subnet, you can use multiple layers of security, including security groups and network access control lists (ACL). 
   - Security Groups
   - Network access control lists (NACL) 
- Accessing from the On-premises Network: You can optionally connect your VPC to your own corporate data center using an IPsec AWS VPN connection.

## Lab Prerequisites

Make sure you've gone through the Workshop prerequisites (Cloud9 IDE, AWS CLI etc.). 

## Expected Costs

VPC, NACL, Security Group (a.k.a. Stateless Firewalls) are free. EC2 is free tier eligible. Internet Gateway is charged based on data out.


## Lab Steps

This lab is based on https://docs.aws.amazon.com/vpc/latest/userguide/vpc-subnets-commands-example.html
 
1. Open the Cloud9 IDE and go to the terminal screen.
1. Create a new VPC which uses 10.0.0.0/16 IP Range. Copy the created VPC-id

    ```sh
    aws ec2 create-vpc --cidr-block 10.0.0.0/16 --region eu-west-1
    ```

1. Create a subnet into the VPC. Use the vpc-id from the previous command

    ```sh
    aws ec2 create-subnet --vpc-id vpc-0bf4e3d58ad59d446 --cidr-block 10.0.1.0/24 --region eu-west-1
    ```

1. Create an Internet Gateway service. 

    ```sh
    aws ec2 create-internet-gateway --region eu-west-1
    ```

1. Attach the internet gateway service to the VPC you've created. Use the vpc-id and igw-id from the previous commands

    ```sh
    aws ec2 attach-internet-gateway --vpc-id vpc-0bf4e3d58ad59d446 --internet-gateway-id igw-0a58fd2eb642483cc --region eu-west-1
    ```

1. [TODO] make sure assign public ip = true


1. Let's create a route table. We'll configure it and attach it to a subnet to control networking.

    ```sh
    aws ec2 create-route-table --vpc-id vpc-0bf4e3d58ad59d446 --region eu-west-1
    ```

1. Let's check what we created

    ```sh
    aws ec2 describe-route-tables --route-table-id rtb-063f9a34f69e63e28 --region eu-west-1
    ```


1. The route table is currently not associated with any subnet. You need to associate it with a subnet in your VPC so that traffic from that subnet is routed to the Internet gateway. First, use the describe-subnets command to get your subnet IDs. You can use the --filteroption to return the subnets for your new VPC only, and the --query option to return only the subnet IDs and their CIDR blocks. Use the vpc-id from the previous commands.

    ```sh
    aws ec2 describe-subnets --filters "Name=vpc-id,Values=vpc-0bf4e3d58ad59d446" --query 'Subnets[*].{ID:SubnetId,CIDR:CidrBlock}' --region eu-west-1
    ```
1. Let's associate the route table with the subnet in your VPC so that traffic from that subnet is routed to the Internet gateway. Use the subnet-id and rtb-id from the previous commands.

    ```sh
    aws ec2 associate-route-table  --subnet-id subnet-08ccfea44f8367711 --route-table-id rtb-063f9a34f69e63e28 --region eu-west-1
    ```

1. Create a new public/private keypair. You will use it to login to your EC2 instance securely via SSH. The second command changes permissions on the key-file. 

    ```sh
    aws ec2 create-key-pair --key-name cbd-izmir-MyKeyPair --query 'KeyMaterial' --output text > cbd-izmir-MyKeyPair.pem --region eu-west-1

    chmod 400 cbd-izmir-MyKeyPair.pem
    ```

1. Create a security group associated with the VPC you created. 

    ```sh
    aws ec2 create-security-group --group-name SSHAccess --description "cbd-izmir Security group for SSH access" --vpc-id vpc-0bf4e3d58ad59d446 --region eu-west-1
    ```

1. Find out your IP

    ```
    http://checkip.amazonaws.com/
    ```

1. Allow access from internet, but only from your current IP ( *Note:* You will need to modify the IP in the secrity group every time your IP changes. But this is a security best practice not to allow access from any internet source IP). Use the sg-id from the previous commands. 

    ```sh
    aws ec2 authorize-security-group-ingress --group-id sg-04a6406da3f9fb848 --protocol tcp --port 22 --cidr <your_current_ip>/32 --region eu-west-1
    ```

1. Create a new instance in your private data center at AWS. Use the sg-id, subnet-id from the previous commands. ( *Note:* The AMI-id in the example below is for EU Ireland Region. if you are using another region, you need to replace it with the corresponding AMI-id. )

    ```sh
    aws ec2 run-instances --image-id ami-08935252a36e25f85 --count 1 --instance-type t2.micro --key-name cbd-izmir-MyKeyPair --security-group-ids sg-04a6406da3f9fb848 --subnet-id subnet-08ccfea44f8367711 --region eu-west-1
    ```
1. Now go to the AWS console -> EC2 and find out the EC2 instance you've created. Check its... 
   - security group attached
   - subnet it is part of

1. Exercise: Find out how to connect to the instance you created.



## Summary

Fantastic, you've created your virtual private DC, configured several networking services (e.g. internet gateway, NACL, stateful firewalls called security groups, and created VM instances in a few commands).  


## Deleting Lab Resources


1. Go to AWS console -> EC2 and find out the EC2 instance you've created. "Terminate the instance from the console"

1. Now let's delete the networking resources. Run the following commands with the correct ids to delete the resources

    ```sh
    aws ec2 delete-security-group --group-id sg-04a6406da3f9fb848  --region eu-west-1

    aws ec2 delete-subnet --subnet-id subnet-08ccfea44f8367711 --region eu-west-1

    aws ec2 delete-route-table --route-table-id rtb-063f9a34f69e63e28 --region eu-west-1

    aws ec2 detach-internet-gateway --internet-gateway-id igw-0a58fd2eb642483cc  --vpc-id vpc-0bf4e3d58ad59d446 --region eu-west-1

    aws ec2 delete-internet-gateway --internet-gateway-id igw-0a58fd2eb642483cc --region eu-west-1

    aws ec2 delete-vpc --vpc-id vpc-0bf4e3d58ad59d446 --region eu-west-1
    ```



