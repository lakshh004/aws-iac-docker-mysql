# 🚀 AWS Infrastructure as Code – Dockerized MySQL on EC2

![AWS](https://img.shields.io/badge/AWS-CloudFormation-orange)
![EC2](https://img.shields.io/badge/EC2-t3.micro-yellow)
![Docker](https://img.shields.io/badge/Docker-Containerized-blue)
![Status](https://img.shields.io/badge/Stack-CREATE_COMPLETE-brightgreen)

---

## 📌 Project Overview

This project demonstrates how to provision complete AWS infrastructure using **CloudFormation (Infrastructure as Code)** and automatically deploy a **Dockerized MySQL database** on EC2 using **UserData bootstrapping**.

Instead of manually creating resources in the AWS console, everything is defined declaratively in a YAML template and deployed via AWS CLI.

The stack provisions:

- Custom VPC  
- Multi-AZ subnet architecture  
- Internet Gateway & route tables  
- Security groups  
- EC2 instance (t3.micro)  
- Automatic Docker installation  
- Automatic MySQL container deployment  

No manual SSH configuration was required after launch.

---

## 📂 Project Structure

```bash
aws-iac-docker-mysql/
│
├── infrastructure.yaml   # CloudFormation template (entire infrastructure)
├── .gitignore            # Prevents private key upload
└── README.md             # Project documentation
