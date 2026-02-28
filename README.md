# 🚀 AWS Infrastructure as Code – Dockerized MySQL on EC2

![AWS](https://img.shields.io/badge/AWS-CloudFormation-orange)
![EC2](https://img.shields.io/badge/EC2-t3.micro-yellow)
![Docker](https://img.shields.io/badge/Docker-Containerized-blue)
![Status](https://img.shields.io/badge/Stack-CREATE_COMPLETE-brightgreen)

---

## Project Overview

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

## Project Structure

```bash
aws-iac-docker-mysql/
│
├── infrastructure.yaml   # CloudFormation template (entire infrastructure)
├── .gitignore            # Prevents private key upload
└── README.md             # Project documentation 
```

---

## Architecture Diagram

<img width="1015" height="469" alt="architecture " src="https://github.com/user-attachments/assets/90a98732-841c-4f87-b618-e6b690d6c54c" />
<p align="center">
  <u>Architecture Diagram</u>
</p>


---

## Architecture Breakdown

###  AWS Region
`us-east-1`

###  VPC Configuration
* **VPC CIDR:** `10.0.0.0/16`

###  Subnet Design
* **Public Subnet (10.0.1.0/24):** Hosts EC2
* **Private Subnet 1 (10.0.2.0/24):** Reserved for backend services
* **Private Subnet 2 (10.0.3.0/24):** Demonstrates multi-AZ design

> The private subnets are not actively used but demonstrate scalable, production-style architecture planning.

---

##  Compute Layer

###  EC2 Instance (t3.micro)
Launched inside the public subnet with:

**Security Group allowing:**
* Port 22 (SSH)
* Port 80 (HTTP)
* Port 3306 (MySQL)
* **IAM role** attached
* **CloudWatch** monitoring enabled

###  Automated Docker Deployment
The EC2 instance uses **UserData** to automatically install Docker and run a MySQL container during instance boot.

```bash
#!/bin/bash
yum update -y
amazon-linux-extras install docker -y
systemctl start docker
systemctl enable docker
usermod -aG docker ec2-user

docker run -d \
  --name mysql-container \
  -e MYSQL_ROOT_PASSWORD=StrongPass123 \
  -p 3306:3306 \
  mysql:8
```
**When the instance boots:**

* Docker installs automatically.
* MySQL 8 container starts automatically.
* Port 3306 is exposed.
* **No manual configuration** is required after stack creation.

---

## Security Considerations
* Infrastructure created only via CloudFormation.
* No manual console configuration.
* Security group restricts inbound ports.
* MySQL public access enabled for demonstration purposes only.
* Architecture supports future migration of database to a private subnet.

---

## Deployment & Verification

### 1. Validate Template
```bash
aws cloudformation validate-template \
  --template-body file://infrastructure.yaml
```
### 2. Create Stack
```bash
aws cloudformation create-stack \
  --stack-name iac-stack \
  --template-body file://infrastructure.yaml \
  --parameters ParameterKey=KeyPairName,ParameterValue=<your-key-name>
```
### 3. Monitor Stack Status
```bash
aws cloudformation describe-stacks \
  --stack-name iac-stack \
  --query "Stacks[0].StackStatus"
```
**Wait until:** `CREATE_COMPLETE`

### 4. Verify Docker and MySQL
SSH into the EC2 instance and run:
```bash
docker ps
```
**This confirms:**
* MySQL container running
* Port mapping active
* Docker engine operational

## Project Screenshots
<img width="905" height="535" alt="Create Complete" src="https://github.com/user-attachments/assets/b5cce2fe-1801-42d1-a2d7-34cd96acca31" />
<p align="center">
  <u>CloudFormation "CREATE COMPLETE" screenshot.</u>
</p>
<br> </br>

 <img width="1197" height="595" alt="ec-2 running" src="https://github.com/user-attachments/assets/a8328905-62f3-43f7-95ab-397fc3bf277c" />
<p align="center">
  <u>EC2 Running instance screenshot.</u>
</p>

<img width="1105" height="51" alt="docker ps" src="https://github.com/user-attachments/assets/36a0ad6e-9004-4fe3-8941-573cf6480471" />
<p align="center">
  <u>docker ps output screenshot.</u>
</p>

*<img width="1074" height="588" alt="resources" src="https://github.com/user-attachments/assets/f03f1cdf-68e7-470f-ae24-299c1998c43c" />
<p align="center">
  <u>Resources tab screenshot.</u>
</p>

<img width="1457" height="573" alt="user data" src="https://github.com/user-attachments/assets/da6f08cc-8b31-4fef-918d-f68158657dff" />
<p align="center">
  <u>UserData configuration screenshot.</u>
</p>


## What This Project Demonstrates
* Infrastructure as Code (IaC) principles
* AWS networking fundamentals
* Multi-AZ subnet design
* Automated instance bootstrapping
* Containerized service deployment
* CLI-driven infrastructure provisioning
* Reproducible cloud environments

  ---

## Conclusion

This project helped me move beyond just learning AWS concepts and actually implement them in a structured, reproducible way. Instead of manually configuring resources in the console, I defined everything declaratively and let CloudFormation handle provisioning.

Working through stack failures, rollback errors, networking misconfigurations, and UserData debugging gave me practical insight into how AWS behaves in real scenarios. It reinforced the importance of automation, clean architecture design, and infrastructure reproducibility.

More than just deploying a MySQL container, this project strengthened my understanding of how infrastructure, networking, compute, and containers connect together in a cloud environment.

It reflects a core DevOps mindset:
automate everything, avoid manual steps, and build systems that can be recreated reliably at any time.







