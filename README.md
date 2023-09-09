# Project Title: Deploying Highly Available and Scalable Application in a VPC

# Project Overview

The objective of this project is to deploy a highly available and scalable application within a Virtual Private Cloud (VPC). This involves designing the VPC architecture, configuring subnets, implementing load balancing, auto scaling, and setting up a database for the application. The project aims to ensure the application can handle increased traffic and maintain uptime even during failures.

## Objectives

1. **Design a VPC with appropriate network segmentation.**
2. **Configure subnets for availability and security.**
3. **Implement load balancing to distribute incoming traffic.**
4. **Set up auto scaling to handle traffic fluctuations.**
5. **Establish a database for data storage.**
6. **Test and validate high availability, scalability, and failover mechanisms.**


## Table of Contents
1. [VPC Configuration](#vpc-configuration)
2. [Launch Template](#launch-template)
3. [Auto Scaling Group (ASG)](#auto-scaling-group-asg)
4. [Target Group](#target-group)
5. [Load Balancer (Application Load Balancer)](#load-balancer-application-load-balancer)
6. [Bastion Host](#bastion-host)
7. [Install NGINX on Server using Bastion Host](#install-nginx-on-server-using-bastion-host)
8. [Access Application from Browser](#access-application-from-browser)

## VPC Configuration

1. **Create VPC**:
   - VPC Name: [Your VPC Name]
   - IPv4 CIDR block: [Your CIDR block, e.g., 10.0.0.0/16]
   - Number of Availability Zones (AZs): [Number]
   - Number of public subnets: [Number]
   - Number of private subnets: [Number]
   - NAT gateways (1 per AZs): [Number]

2. **Internet Gateway**:
   - Ensure Internet Gateway is created for VPC.
   - If not, create it and attach it to the VPC.

3. **Route Table Configuration**:
   - For associated Public Subnet:
     - Configure: Destination 0.0.0.0/0 & target: Internet Gateway.

   - For each associated Private Subnet:
     - Configure: Destination 0.0.0.0/0 & target: NAT Gateway.
     - Note: The default route for target local should be present and should not be edited.

## Launch Template

4. **Launch Template Configuration**:
   - Select OS AMI
   - Choose Instance Type
   - Select KeyPair for SSH
   - Subnet: "Don't include in launch template"
   - Create a new Security Group
   - Select the VPC
   - Your Launch Template is now ready for use.

## Auto Scaling Group (ASG)

5. **Auto Scaling Group Configuration**:
   - Select Launch Template (previously configured)
   - Select VPC & Private Subnets in different AZs
   - Enter desired, minimum & maximum capacity
   - Define Scaling Policy
   - Add Notification (Create SNS Topic & Select it)
   - Your ASG will be ready.

## Target Group

6. **Target Group Configuration**:
   - Select Target Type as Instance
   - Select Port (HTTP port: 80)
   - Select VPC
   - Protocol version: HTTP1
   - Health Checks (Leave as it is)
   - Select Instances & move to Include as pending
   - Create the Target Group.

## Load Balancer (Application Load Balancer)

7. **Load Balancer Configuration**:
   - Select Load Balancer
   - Select Internet Facing
   - Select VPC, Public Subnets
   - Create a Security Group that allows HTTP traffic from the internet (HTTP, 0.0.0.0/0) and select it
   - Select the created Target Group for Listeners & Routing
   - Click Create Load Balancer.

## Bastion Host

8. **Bastion Host Configuration**:
   - Create an EC2 Instance in the same VPC & public subnet.
   - Enable auto assign Public IP.

## Install NGINX on Server using Bastion Host

9. **Install NGINX on Private Subnet Instances**:
   - SSH to the Bastion Host Instance.
   - Create a file for KeyPair (e.g., `vi Key.pem`) & add the attached KeyPair of the Launch Instance.
   - SSH to Private Subnet Instances.
   - Run `sudo apt-get install nginx -y` to install NGINX.
   - Create a sample HTML file.
   - Copy it to the `/var/www/html` directory.

## Access Application from Browser

10. **To access the application from a browser**:
    - Copy the DNS of the Load Balancer and paste it in your browser.
