🚀 Day 28 – Three-Tier Highly Available AWS Architecture with Terraform
📌 Project Overview

This project implements a production-grade, highly available three-tier architecture on AWS using Terraform.

It demonstrates how to design and provision scalable, fault-tolerant infrastructure following AWS best practices, including:

Multi-AZ deployment

Public and private subnet segregation

Internet-facing and internal load balancers

Auto Scaling Groups

Dockerized applications on EC2

RDS database in Multi-AZ mode

Bastion host for secure access

NAT Gateway for outbound internet access

AWS Secrets Manager for credential management

This project is designed to simulate real-world production infrastructure.

🏗 Architecture Overview
🔹 Tier 1 – Presentation Layer (Frontend)

EC2 instances deployed in private subnets

Dockerized frontend application

Internet-facing Application Load Balancer

Auto Scaling Group across multiple AZs

🔹 Tier 2 – Logic Layer (Backend)

EC2 instances in private subnets

Dockerized backend service

Internal Application Load Balancer

Secure communication from frontend only

🔹 Tier 3 – Database Layer

RDS (PostgreSQL) in Multi-AZ mode

DB subnet group in private subnets

Credentials managed via AWS Secrets Manager

No public exposure

🔐 Secure Access

Bastion Host in public subnet

SSH access only from trusted IP

Used to access private EC2 instances

🌐 Networking Flow

User → Internet

Internet → Internet Gateway

Internet Gateway → External ALB

External ALB → Frontend ASG

Frontend → Internal ALB

Internal ALB → Backend ASG

Backend → RDS

🗂 Project Structure
terraform/
│
├── modules/
│   ├── vpc/
│   ├── security-groups/
│   ├── bastion/
│   ├── frontend/
│   ├── backend/
│   ├── rds/
│   └── alb/
│
├── env/
│   └── dev/
│       ├── main.tf
│       ├── variables.tf
│       └── terraform.tfvars
│
└── provider.tf


Each module encapsulates a specific infrastructure component for modularity and reusability.

🔐 Security Design

EC2 instances are placed in private subnets

Only ALB is exposed publicly

Bastion host is the only SSH entry point

Strict Security Group rules:

ALB → Frontend (Port 3000)

Frontend → Backend (Port 8080)

Backend → RDS (Port 5432)

NAT Gateway enables outbound internet access for private instances

IAM roles provide least-privilege access

Database credentials stored securely in Secrets Manager

📦 Docker Deployment

Frontend and Backend applications are containerized.

Build and Push Docker Images
docker build -t your-dockerhub-username/frontend .
docker push your-dockerhub-username/frontend

docker build -t your-dockerhub-username/backend .
docker push your-dockerhub-username/backend


EC2 user-data scripts automatically pull and run these images.

🚀 Deployment Steps
1️⃣ Initialize Terraform
terraform init

2️⃣ Validate Configuration
terraform validate

3️⃣ Plan Infrastructure
terraform plan

4️⃣ Apply Infrastructure
terraform apply


Provisioning may take time (RDS creation especially).

✅ Post-Deployment Verification

Check External ALB DNS output

Verify Target Group health checks

Confirm Auto Scaling instances are running

SSH into Bastion → Access private EC2

Verify RDS connectivity from backend

Confirm internal ALB routing works correctly

📈 High Availability & Scalability

Deployed across multiple Availability Zones

Auto Scaling Groups configured with:

Minimum capacity

Desired capacity

Maximum capacity

Health checks ensure unhealthy instances are replaced automatically

RDS Multi-AZ replication ensures database failover

⚠️ Common Troubleshooting
RDS Parameter Errors

Verify engine version compatibility

Check Terraform AWS provider documentation

Health Check Failures

Confirm application is listening on correct port

Verify Security Group rules

Docker Pull Failures

Ensure NAT Gateway is correctly configured

Confirm outbound internet access from private subnet

💰 Cost Awareness

This architecture includes:

2 Load Balancers

Multiple EC2 instances

NAT Gateway

Multi-AZ RDS

These resources incur significant cost.

Always destroy infrastructure after testing:

terraform destroy

🎯 Key Learning Outcomes

Designing production-grade AWS architecture

Implementing three-tier infrastructure

Securing private subnets properly

Using modular Terraform structure

Managing secrets securely

Deploying Docker containers on EC2

Debugging real-world Terraform provisioning issues

🧠 Why This Project Matters

This project simulates real enterprise infrastructure patterns used in production environments.

It strengthens understanding of:

Networking design

Load balancing strategies

Autoscaling behavior

Secure database connectivity

Infrastructure as Code best practices

🧹 Cleanup

After testing:

terraform destroy


Ensure:

RDS instances are deleted

NAT Gateway is removed

Elastic IPs are released

🏁 Conclusion

This implementation demonstrates how to build a secure, scalable, and highly available three-tier architecture using Terraform on AWS.

It bridges theory and real-world infrastructure engineering practices and reinforces essential cloud architecture principles through hands-on deployment.
