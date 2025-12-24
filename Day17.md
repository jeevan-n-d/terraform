# 🚀 Terraform Day 17: Blue-Green Deployment on AWS Using Terraform

Day 17 focuses on implementing **Blue-Green Deployment** on AWS using **Terraform** and **Elastic Beanstalk**.  
This project demonstrates how to deploy application updates with **minimal downtime**, enable **safe releases**, and perform **instant rollbacks** using infrastructure as code.

---

## 🎯 Objective

The goal of this project is to:

- Automate **blue-green deployment** using Terraform
- Deploy application versions safely without impacting users
- Switch production traffic seamlessly between environments
- Enable fast rollback in case of failure

This setup mirrors **real-world production deployment strategies**.

---

## 🧠 What Is Blue-Green Deployment?

Blue-Green deployment maintains **two identical environments**:

- **Blue** → Current live production
- **Green** → New version (idle / staging)

Deployment flow:

1. Deploy new version to **Green**
2. Test Green thoroughly
3. Swap traffic from **Blue → Green**
4. Roll back instantly if needed by swapping again

This approach ensures:
- Near-zero downtime
- Safe releases
- Simple rollback

---

## 🏗 Architecture Overview

The project provisions:

- **S3 Bucket** – stores application artifacts (ZIP files)
- **Elastic Beanstalk Application**
- **Two Elastic Beanstalk Environments**
  - Blue (Production)
  - Green (Staging / New version)
- **IAM Roles & Policies**
- **Terraform** to manage the entire lifecycle

Both environments are **identical in configuration** — only the application version differs.

---

## 📂 Project Structure

```text
day17/
├── main.tf
├── variables.tf
├── outputs.tf
├── providers.tf
├── app/
│   ├── v1/
│   │   └── app_v1.zip
│   └── v2/
│       └── app_v2.zip
└── scripts/
    ├── package_app.sh
    └── package_app.ps1
📦 Application Packaging
Application source code is packaged into versioned ZIP files:

app_v1.zip → deployed to Blue

app_v2.zip → deployed to Green

Packaging scripts are provided for:

Linux / macOS

Windows

Elastic Beanstalk pulls these artifacts from S3 during deployment.

⚙️ Terraform Deployment Workflow
Run the standard Terraform commands:

bash
Copy code
terraform init
terraform plan
terraform apply
After apply:

Blue environment runs version 1.0

Green environment runs version 2.0

Both environments are live and accessible via separate URLs

🔁 Traffic Swapping (Blue ↔ Green)
Traffic switching is done by swapping Elastic Beanstalk environment CNAMEs.

What happens during a swap:

DNS entries are updated

Traffic moves to the new environment

No redeployment occurs

Minimal or zero downtime

From the user perspective:

URL remains the same

Application version changes seamlessly

🔙 Rollback Strategy
Rollback is instant:

Perform another environment swap

Traffic returns to the previous stable version

No rebuilds. No redeployments. No downtime.

🧠 Key Learnings
Blue-Green deployment minimizes release risk

Terraform enables repeatable, auditable deployments

Elastic Beanstalk simplifies application hosting

S3-based versioning supports controlled releases

DNS-based swapping ensures seamless traffic transitions

Rollbacks become fast and safe

⚠️ Cleanup (Important)
To avoid AWS charges, destroy resources after testing:

bash
Copy code
terraform destroy
🏁 Conclusion
This project demonstrates how Terraform can be used beyond infrastructure provisioning — enabling safe, production-grade deployment workflows.

Blue-Green deployment combined with Terraform provides:

Reliability

Release confidence

Operational safety

This is a real-world DevOps deployment pattern, not a demo-only concept.
