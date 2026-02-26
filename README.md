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
