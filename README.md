# 3-Tier Architecture with VPC

# Objective

Design and deploy a secure 3-tier application architecture on AWS consisting of:  
Web tier (Application Load Balancer in public subnets)  
App tier (EC2 instances in private subnets)  
Database tier (Amazon RDS in private subnets)  

# Architecture Diagram

![arch](https://github.com/vibincholayil/3-Tier-Architecture-with-VPC/blob/master/images/arch.png?raw=true)  

# VPC & Networking

### Create VPC  
![1](https://github.com/vibincholayil/3-Tier-Architecture-with-VPC/blob/master/images/1.png?raw=true)  
![2](https://github.com/vibincholayil/3-Tier-Architecture-with-VPC/blob/master/images/2.png?raw=true)  
### VPC Created
![3](https://github.com/vibincholayil/3-Tier-Architecture-with-VPC/blob/master/images/3.png?raw=true)  

![4](https://github.com/vibincholayil/3-Tier-Architecture-with-VPC/blob/master/images/4.png?raw=true)


![5](https://github.com/vibincholayil/3-Tier-Architecture-with-VPC/blob/master/images/5.png?raw=true)


![6](https://github.com/vibincholayil/3-Tier-Architecture-with-VPC/blob/master/images/6.png?raw=true)


![7](https://github.com/vibincholayil/3-Tier-Architecture-with-VPC/blob/master/images/7.png?raw=true)


![8](https://github.com/vibincholayil/3-Tier-Architecture-with-VPC/blob/master/images/8.png?raw=true)


![9](https://github.com/vibincholayil/3-Tier-Architecture-with-VPC/blob/master/images/1.png?raw=true)


![10](https://github.com/vibincholayil/3-Tier-Architecture-with-VPC/blob/master/images/1.png?raw=true)


Subnets:

Public subnets (in 2 AZs) → ALB + NAT Gateway

Private subnets (in 2 AZs) → EC2 + RDS

Route Tables:

Public subnets route → Internet Gateway

Private subnets route → NAT Gateway

# Components
Web Tier

Application Load Balancer (ALB) in public subnets

Listener on port 80 (HTTP)

Target group → app EC2s in private subnets

App Tier

EC2 instances (Auto Scaling Group optional) in private subnets

Connected to ALB target group

Outbound internet via NAT Gateway

Database Tier

Amazon RDS (MySQL/Postgres) in private subnets

Multi-AZ enabled (recommended)

Accessible only from App EC2s

# Security Groups

ALB SG: Allow inbound HTTP (80) from 0.0.0.0/0

App EC2 SG: Allow inbound only from ALB SG (port 80 or app port)

RDS SG: Allow inbound only from App EC2 SG (port 3306 for MySQL / 5432 for Postgres)

NAT Gateway SG: Outbound internet access for private EC2

# Implementation Tasks

Create VPC, subnets, IGW, NAT Gateway, and route tables.

Deploy ALB in public subnets.

Launch EC2s in private subnets and register with ALB target group.

Deploy RDS in private subnets with proper security group.

Test application flow:

User → ALB (public) → EC2 (private) → RDS (private).

# Security Considerations

Principle of least privilege for security groups and NACLs.

RDS not publicly accessible.

EC2 instances in private subnet only (no public IPs).

Use IAM roles instead of static credentials.


-------------------------------
install php and apache on the app sever
https://docs.aws.amazon.com/linux/al2023/ug/ec2-lamp-amazon-linux-2023.html

Step 1: Connect to App Server

From your jump server (replace <APP-PRIVATE-IP> with the private IP of app server):

ssh -i vckeypair.pem ec2-user@<APP-PRIVATE-IP>

Step 2: Update Packages
sudo dnf update -y

Step 3: Install Apache
sudo dnf install httpd -y


Start Apache:

sudo systemctl start httpd


Enable Apache to start on boot:

sudo systemctl enable httpd


Check Apache status:

sudo systemctl status httpd

Step 4: Install PHP
sudo dnf install php php-mysqlnd -y


Restart Apache to load PHP:

sudo systemctl restart httpd

Step 5: Test PHP

Go to Apache root directory:

cd /var/www/html


Create a PHP test file:

sudo nano info.php


Add:

<?php
phpinfo();
?>


Save and exit.

From a browser, access the app server:

If you have a public IP / port forwarding, go to:

http://<APP-PUBLIC-IP>/info.php


Or if private subnet, access via jump server using SSH port forwarding:

ssh -i vckeypair.pem -L 8080:<APP-PRIVATE-IP>:80 ec2-user@<JUMP-PUBLIC-IP>


Then open in browser: http://localhost:8080/info.php




