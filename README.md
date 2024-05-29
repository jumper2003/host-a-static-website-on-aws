# Host a Static Website on AWS

This project involves hosting a static HTML web application on AWS, utilizing various AWS resources to ensure scalability, reliability, and security. Below are the detailed steps and configurations used to deploy the web application on an EC2 instance within a Virtual Private Cloud (VPC). The reference diagram and scripts used in this project are available in the [GitHub repository](https://github.com/aosnotes77/host-a-static-website-on-aws).

## Table of Contents
- [Architecture Overview](#architecture-overview)
- [Prerequisites](#prerequisites)
- [Deployment Steps](#deployment-steps)
- [Configuration Details](#configuration-details)
- [Scripts](#scripts)
- [Resources](#resources)

## Architecture Overview
1. **Virtual Private Cloud (VPC):** Configured with both public and private subnets across two different availability zones.
2. **Internet Gateway:** Facilitates connectivity between VPC instances and the wider Internet.
3. **Security Groups:** Serve as a network firewall mechanism.
4. **Availability Zones:** Enhance system reliability and fault tolerance by leveraging multiple zones.
5. **Public Subnets:** Used for infrastructure components like the NAT Gateway and Application Load Balancer.
6. **EC2 Instance Connect Endpoint:** Allows secure connections to assets within both public and private subnets.
7. **Private Subnets:** Host web servers (EC2 instances) for enhanced security.
8. **NAT Gateway:** Enables instances in private subnets to access the Internet.
9. **Application Load Balancer:** Distributes web traffic evenly to an Auto Scaling Group of EC2 instances.
10. **Auto Scaling Group:** Automatically manages EC2 instances to ensure website availability, scalability, and fault tolerance.
11. **GitHub:** Stores web files for version control and collaboration.
12. **Certificate Manager:** Secures application communications.
13. **Simple Notification Service (SNS):** Alerts about activities within the Auto Scaling Group.
14. **Route 53:** Manages domain name and DNS records.

## Prerequisites
- AWS Account
- AWS CLI configured
- Git installed on your local machine
- A domain name registered in Route 53

## Deployment Steps

1. **Configure VPC:**
   - Create a VPC with public and private subnets across two availability zones.
   - Set up an Internet Gateway and attach it to the VPC.
   - Establish Security Groups for controlling traffic.

2. **Deploy Infrastructure Components:**
   - Set up a NAT Gateway in the public subnet.
   - Configure an Application Load Balancer in the public subnet.

3. **Launch EC2 Instances:**
   - Deploy EC2 instances in private subnets.
   - Use EC2 Instance Connect for secure SSH access.

4. **Set Up Auto Scaling Group:**
   - Create an Auto Scaling Group to manage EC2 instances across multiple availability zones.
   - Configure scaling policies based on traffic patterns.

5. **Secure Communications:**
   - Use AWS Certificate Manager to provision SSL/TLS certificates.
   - Configure the Application Load Balancer to use these certificates.

6. **Set Up Notifications:**
   - Use Amazon SNS to set up notifications for Auto Scaling Group activities.

7. **Configure DNS:**
   - Use Route 53 to manage your domain name and DNS records.

## Configuration Details

### VPC Configuration
- **Subnets:** Create public and private subnets in two availability zones.
- **Internet Gateway:** Attach to VPC for internet access.
- **NAT Gateway:** Place in public subnet for instances in private subnets to access the internet.

### Security Groups
- Configure rules to allow HTTP/HTTPS traffic to the Application Load Balancer.
- Allow SSH traffic from a trusted IP range to the EC2 instances.

### EC2 Instances
- **AMI:** Use an Amazon Linux 2 AMI.
- **Instance Type:** Select an appropriate instance type (e.g., t2.micro for small-scale applications).
- **User Data Script:** Use the provided script to install Apache, clone the GitHub repository, and deploy the web application.

### Auto Scaling Group
- Configure to maintain the desired number of instances.
- Set policies to scale based on CPU utilization or other metrics.

### Application Load Balancer
- Distribute traffic across EC2 instances in different availability zones.
- Attach SSL/TLS certificate for secure communications.

### Route 53
- Create a hosted zone for your domain.
- Set up DNS records to point to the Application Load Balancer.

## Scripts

### User Data Script for EC2 Instances
```bash
#!/bin/bash
# Switch to the root user to gain full administrative privileges
sudo su
# Update all installed packages to their latest versions
yum update -y
# Install Apache HTTP Server
yum install -y httpd
# Change the current working directory to the Apache web root
cd /var/www/html
# Install Git
yum install git -y
# Clone the project GitHub repository to the current directory
git clone https://github.com/aosnotes77/host-a-static-website-on-aws.git
# Copy all files, including hidden ones, from the cloned repository to the Apache web root
cp -R host-a-static-website-on-aws/. /var/www/html/
# Remove the cloned repository directory to clean up unnecessary files
rm -rf host-a-static-website-on-aws
# Enable the Apache HTTP Server to start automatically at system boot
systemctl enable httpd
# Start the Apache HTTP Server to serve web content
systemctl start httpd
```

## Resources
- [AWS VPC Documentation](https://docs.aws.amazon.com/vpc)
- [AWS EC2 Documentation](https://docs.aws.amazon.com/ec2)
- [AWS Auto Scaling Documentation](https://docs.aws.amazon.com/autoscaling)
- [AWS Certificate Manager Documentation](https://docs.aws.amazon.com/acm)
- [Amazon Route 53 Documentation](https://docs.aws.amazon.com/route53)

For detailed instructions and the reference diagram, please visit the [GitHub repository](https://github.com/aosnotes77/host-a-static-website-on-aws).

---
