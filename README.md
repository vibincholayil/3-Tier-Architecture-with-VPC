# Three-Tier Application using AWS

This repository contains Frontend, Backend, Database code for build a three-tier architecture. The application demonstrates the fundamental concepts of separating frontend, backend, and database components in a web application.

## Architecture Overview

![alt text](images/aws-three-tier-architecture.png)

The application follows the classic three-tier architecture:

1. **Frontend Tier (Presentation Layer)**
   - HTML/CSS/JavaScript
   - Served by NGINX web servers
   - Handles user interface and interactions

2. **Backend Tier (Application Layer)**
   - PHP API
   - Processes business logic
   - Communicates with database
   - Serves data to frontend

3. **Database Tier (Data Layer)**
   - MySQL database
   - Stores application data
   - Provides data persistence

## Features

- Display messages from the database
- Add new messages to the database
- Basic responsive design

## Web Output
![alt text](images/output.png)

## AWS Infrastructure Components

When deployed on AWS, the infrastructure includes:

- **Web ALB**: Load balancer for distributing traffic to web servers
- **NGINX Servers**: EC2 instances in an auto-scaling group
- **App ALB**: Load balancer for distributing traffic to application servers
- **PHP Servers**: EC2 instances in an auto-scaling group
- **RDS MySQL**: Managed relational database service

## Directory Structure

```
three-tier-architecture-aws/
├── frontend/
│   ├── index.html            # Main HTML file
│   ├── styles.css            # CSS styles
│
├── backend/
│   └── api/                  # API endpoints
│       ├── get_messages.php  # API to retrieve messages
│       ├── save_message.php  # API to save new messages
│       └── db_connection.php # Database connection utility
│
├── database/
│   └── database_setup.sql    # SQL schema and initial data
│
└── infrastructure/           # AWS infrastructure configurations
    ├── frontend_server.md     # Frontend server configurations
    ├── backend_server.md     # Backend server configurations
    ├── nginx_config     # Nginx server configurations
```

## Local Setup

### Prerequisites

- Web server with PHP support (XAMPP, WAMP, MAMP, etc.)
- MySQL database

### Steps

#### Create VPC
![alt text](images/1.create_vpc.png)
![alt text](images/2_vpc_details.png)

#### Create subnets
    1. Web Public 1a, 1b, 1c
    2. Web Private 1a, 1b, 1c
    3. App Private 1a, 1b, 1c
    4. Db Private 1a, 1b, 1c
![alt text](images/3_create_subnet.png)

#### Create route tables
    1. Web Public
    2. Web Private 1a, 1b, 1c
    3. App Private 1a, 1b, 1c
    4. Db Private 1a, 1b, 1c
![alt text](images/4_route_tables.png)

#### Associate route tables with subnet
![alt text](images/5_assosiated_web_public_subnet)
All other route tables are associated with its relevant subnets.

#### Create internet Gateway (IGW)
    1. Attach it to VPC
![alt text](images/6_igw.png)

#### Create NAT gateway (NATGW) in web public subnet
![alt text](images/7_natgw.png)

#### Add IGW and NAT routes in route table
    1. Public -> IGW
![alt text](images/8_add_igw_rt.png)
    2. Private -> NAT
![alt text](images/9_add_natgw_rt.png)

#### Create security groups
    1. Frontend ALB
    2. Frontend Servers
    3. Backend ALB
    4. Backend Servers
    5. Db Private Servers
![alt text](images/10_sg.png)

#### Create database subnet group
![alt text](images/11_db_subnetgroup.png)

#### Create database server
![alt text](images/12_db_server.png)

#### Create Frontend ALB
![alt text](images/13_forntend_alb.png)
    1. Create Frontend ALB target group 

#### Create Backend ALB
![alt text](images/14_backend_alb.png)
    1. Create Backend ALB target group

#### Create Frontend Server AMI
![alt text](images/15_frontend_server_ami.png)
    1. Install Nginx
    2. Install Git

#### Create Backend Server AMI
![alt text](images/16_backend_server_ami.png)
    1. Install PHP, MySQL, Apache
    2. Install Git
    3. Run the database script
'''
-- database_setup.sql  

-- Create database  
CREATE DATABASE IF NOT EXISTS hello_world;  
USE hello_world;  

-- Create messages table  
CREATE TABLE IF NOT EXISTS messages (  
    id INT AUTO_INCREMENT PRIMARY KEY,  
    message VARCHAR(255) NOT NULL,  
    created_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP  
);  

-- Insert some initial data  
INSERT INTO messages (message) VALUES   
('Hello from the database!'),  
('Welcome to our three-tier architecture demo'),  
('This is a simple example showing frontend, backend, and database');  
'''

#### Create the Launch Template for Frontend Server
#### Create the Launch Template for Backend Server
![alt text](images/17_lt.png)

#### Create the Auto Scaling Group for Frontend Server
#### Create the Auto Scaling Group for Backend Server
![alt text](images/18_asg.png)

## Development

### Frontend Development

The frontend is built with plain HTML, CSS, and JavaScript. It uses the Fetch API to communicate with the backend.

To make changes to the frontend:
1. Modify the HTML/CSS/JavaScript files in the `frontend` directory
2. Test the changes locally

![alt text](images/19_frontend_server.png)

### Backend Development

The PHP backend provides simple API endpoints for retrieving and saving messages.

To make changes to the backend:
1. Modify the PHP files in the `backend/api` directory
2. Test the changes locally

![alt text](images/20_backend_server.png)

## Security Considerations

This is a demo application and lacks several security features that would be necessary in a production environment:

- Input validation and sanitization
- Authentication and authorization
- HTTPS encryption
- Protection against SQL injection (although PDO with prepared statements is used)
- CORS configuration

## Conclusion
This project showcases a simple three-tier application on AWS, demonstrating the separation of frontend, backend, and database layers. The architecture provides scalability, reliability, and a strong foundation for learning cloud deployments, DevOps practices, and infrastructure automation.






