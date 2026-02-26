# AWS VPC + EC2 + Docker (Nginx) Lab

This project is a simple end-to-end setup on AWS to learn core DevOps fundamentals.  
I created a custom VPC, deployed an EC2 instance inside a public subnet, installed Docker on it, and used Nginx to serve a basic web page.

The aim of the project was to understand:
- how VPC networking works,
- how public subnets and route tables allow internet access,
- how to secure an EC2 instance,
- and how to deploy a containerised web server.

---

## Architecture Overview

The setup looks like this:

```
VPC (10.0.0.0/16)
│
├── Internet Gateway
│
├── Public Route Table (0.0.0.0/0 → IGW)
│
├── Public Subnet A (10.0.1.0/24)
│   └── EC2 Instance (Amazon Linux 2023)
│       └── Docker → Nginx → index.html
│
└── Public Subnet B (10.0.2.0/24)
```
---

## What I Built

This lab taught me how the main networking pieces in AWS fit together and how to deploy a simple containerised web server. Here’s a breakdown of the components I created:

### 1. Custom VPC
- CIDR block: **10.0.0.0/16**
- This gave me my own isolated network to work inside.

### 2. Internet Gateway
- Attached to the VPC so anything inside the network could reach the internet (when allowed).

### 3. Public Route Table
- Contains a route: **0.0.0.0/0 → Internet Gateway**
- This is what allows resources in the public subnet to get internet access.
- I associated this route table with Public Subnet A.

### 4. Public Subnet A
- CIDR: **10.0.1.0/24**
- This is where I launched my EC2 instance.  
- Because it’s a public subnet, the instance can receive a public IP and be reachable from my browser.

### 5. EC2 Instance (Amazon Linux 2023)
- Auto-assign public IP: enabled  
- Security Group:
  - **SSH (22)** → locked down to **my IP only**
  - **HTTP (80)** → open to the internet so the web page is reachable
- This instance is where I installed Docker and served my test website.

### 6. Docker + Nginx
After connecting to my EC2 instance, I installed Docker and used an Nginx container to host my custom `index.html` page:

```bash
sudo yum update -y
sudo yum install docker -y
sudo systemctl start docker

sudo docker run -d -p 80:80 \
  --name website \
  -v $(pwd)/index.html:/usr/share/nginx/html/index.html \
  nginx
