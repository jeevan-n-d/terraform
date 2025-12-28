# 🚀 Terraform Day 22: Secure Two-Tier Architecture on AWS (EC2 + RDS)

Day 22 focuses on building a **real-world two-tier application architecture on AWS using Terraform**.

This project demonstrates how to deploy:
- A **Flask web application** on EC2 (public subnet)
- A **MySQL RDS database** in private subnets
- **Secure credential management** using AWS Secrets Manager
- **Custom Terraform modules** for clean, reusable infrastructure code

This is a **production-style architecture**, not a demo setup.

---

## 🧠 What This Project Covers

- Designing a **two-tier AWS architecture**
- Using **Terraform custom modules**
- Secure handling of **database credentials**
- Network isolation using **public & private subnets**
- EC2 → RDS connectivity using **security groups**
- Automated application deployment using **EC2 user data**
- Passing values between modules using **Terraform outputs**

---

## 🏗️ Architecture Overview

### Web Tier
- EC2 instance
- Public subnet
- Accessible via public DNS
- Runs a Flask application
- Controlled via security groups

### Database Tier
- MySQL RDS instance
- Private subnets
- No public access
- Accepts traffic only from web tier security group

### Secrets Management
- Database username & password generated dynamically
- Stored securely in AWS Secrets Manager
- Retrieved at runtime by EC2 user data

---

## 📦 Terraform Module Structure

.
├── main.tf # Root module orchestration
├── variables.tf
├── outputs.tf
├── modules/
│ ├── vpc/
│ │ ├── main.tf
│ │ ├── variables.tf
│ │ └── outputs.tf
│ ├── security-groups/
│ │ ├── main.tf
│ │ └── variables.tf
│ ├── secrets/
│ │ ├── main.tf
│ │ └── outputs.tf
│ └── rds/
│ ├── main.tf
│ ├── variables.tf
│ └── outputs.tf

yaml
Copy code

Each module is isolated and reusable.  
The **root module coordinates all dependencies**.

---

## 🔐 Secure Credential Handling

✔ Database password generated using `random_password`  
✔ Stored in **AWS Secrets Manager**  
✔ No hardcoded credentials in Terraform code  
✔ No credentials in variables or user data scripts  

This follows **production security best practices**.

---

## ⚙️ Application Deployment

The EC2 instance uses **user data** to:

- Install system dependencies
- Install Python & Flask
- Fetch DB credentials from Secrets Manager
- Configure environment variables
- Start the Flask application automatically

Result:
> Infrastructure + application deploy together — fully automated.

---

## 🚀 How to Deploy

```bash
terraform init
terraform plan
terraform apply
Notes:

RDS creation takes several minutes (expected)

Public DNS output provides access to the Flask app

Always destroy resources after testing to avoid costs

🧹 Cleanup
bash
Copy code
terraform destroy
⚠️ Ensure RDS and EC2 resources are deleted to prevent billing.

✅ Key Learnings
Two-tier architecture is the baseline for real applications

Databases must live in private subnets

Secrets must be managed securely

Terraform modules simplify complex systems

Output variables enable safe inter-module communication

User data enables zero-touch app deployment

🏁 Conclusion
Day 22 demonstrates how Terraform is used to build real application infrastructure, not just individual resources.

This project brings together:

Networking

Security

Secrets management

Application automation

Modular Terraform design
