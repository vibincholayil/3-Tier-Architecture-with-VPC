# 3-Tier Architecture with VPC

# Objective

Design and deploy a secure 3-tier application architecture on AWS consisting of:  
Web tier (Application Load Balancer in public subnets)  
App tier (EC2 instances in private subnets)  
Database tier (Amazon RDS in private subnets)  

# Architecture Diagram

(Insert your diagram here — show VPC, subnets, ALB, NAT Gateway, EC2, RDS, routing.)

# VPC & Networking

Custom VPC with CIDR block (e.g., 10.0.0.0/16)

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
