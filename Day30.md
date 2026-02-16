# Day 30 – Terraform Drift Detection & Auto-Remediation with GitHub Actions 🚀

## 📌 Project Overview

Day 30 concludes the 30 Days of AWS Terraform challenge with a **production-grade drift detection and auto-remediation system** using:

- Terraform
- GitHub Actions (CI/CD)
- AWS (VPC, ASG, NAT, etc.)
- Slack Notifications
- GitHub Issues Automation

This project ensures that **Terraform remains the single source of truth** by automatically detecting and fixing infrastructure drift.

---

## 🎯 Objectives

- Provision AWS infrastructure using Terraform
- Detect configuration drift automatically
- Remediate drift using CI/CD pipelines
- Send Slack alerts when drift occurs
- Create and manage GitHub issues for audit tracking
- Support multiple environments (dev & prod)

---

## 🏗 Architecture

Infrastructure includes:

- VPC (Public & Private Subnets)
- NAT Gateway
- Auto Scaling Group across AZs
- Separate S3 backend for dev & prod
- DynamoDB for state locking

Terraform state is stored remotely in:
- **S3 bucket**
- **DynamoDB table (for locking)**

---
## 📂 Repository Structure
.
├── terraform/
│ ├── main.tf
│ ├── variables.tf
│ ├── outputs.tf
│ └── backend.tf
│
├── .github/workflows/
│ ├── terraform.yml
│ ├── drift-detection.yml
│ └── destroy.yml
│
└── env/
├── dev.tfvars
└── prod.tfvars



---

## ⚙️ Workflows Implemented

### 1️⃣ Provisioning Workflow (`terraform.yml`)

Triggered on:
- Push to branch
- Pull request

Steps:
- Checkout code
- Configure AWS credentials
- Setup Terraform
- Select workspace (dev/prod)
- Run `terraform plan`
- Run `terraform apply`
- Store plan artifacts

---

### 2️⃣ Drift Detection Workflow (`drift-detection.yml`)

Triggered by:
- Manual run
- Scheduled Cron job

Core logic:

```bash
terraform plan -detailed-exitcode
Exit Codes:

0 → No changes

2 → Drift detected

1 → Error

If exit code = 2:

Run terraform apply -auto-approve

Send Slack notification

Update GitHub issue

3️⃣ Destroy Workflow (destroy.yml)

Triggered manually.

Features:

Select environment (dev/prod)

Manual approval (recommended for prod)

Executes terraform destroy

Cleans up all AWS resources

🔄 Drift Detection Flow

Developer manually changes resource in AWS Console

Scheduled GitHub Action runs

terraform plan detects mismatch

Exit code 2 triggers remediation

Terraform re-applies correct configuration

Slack notification sent

GitHub issue logged and closed

Infrastructure automatically returns to desired state.

🔐 Security Best Practices

AWS credentials stored in GitHub Secrets

Slack webhook stored securely

Separate backends for dev and prod

DynamoDB locking enabled

Manual approval required for production changes

🧪 Hands-on Testing

Test drift detection by:

Manually modifying resource tags in AWS Console

Triggering drift detection workflow

Observing:

Plan detects changes

Auto-apply restores state

Slack notification received

GitHub issue updated

💡 Key Learnings

Terraform exit codes enable intelligent CI/CD branching

Drift detection prevents manual configuration risks

CI/CD automation increases infrastructure reliability

Infrastructure as Code must be enforced continuously

State separation is critical for environment safety

🚀 Why This Matters

In production:

Manual changes create inconsistencies

Teams lose track of actual infrastructure state

Debugging becomes difficult

This project ensures:

Automated monitoring

Automatic remediation

Full traceability

Consistent environments

🏁 Final Outcome

By completing Day 30, you have implemented:

✔ Infrastructure provisioning
✔ CI/CD automation
✔ Drift detection
✔ Auto-remediation
✔ Slack integration
✔ GitHub issue tracking
✔ Environment separation

This represents a real-world DevOps production workflow.

🧹 Cleanup

To destroy infrastructure:

Run destroy workflow manually
OR

Execute:

terraform destroy -auto-approve


⚠️ Always clean up resources to avoid AWS charges.

🎉 30 Days Completed

From basic Terraform commands to full production-grade automation with drift remediation — this journey covers:

Infrastructure as Code

Modules

CI/CD

GitOps

Monitoring

Governance

Cloud Architecture

This project demonstrates production-level Terraform and DevOps capability.
## 📂 Repository Structure

