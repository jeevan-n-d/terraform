# 🚀 Terraform Day 27: CI/CD Automation with GitHub Actions (Multi-Environment)

Day 27 focuses on **automating Terraform workflows using GitHub Actions** to manage AWS infrastructure in a **real-world, multi-environment setup**.

This session demonstrates how Terraform moves from manual CLI execution to a **fully automated CI/CD pipeline** with validation, security scanning, approvals, and controlled destruction.

This is how Terraform is used in professional DevOps teams.

---

## 🎯 What This Project Covers

✔ Terraform automation using GitHub Actions  
✔ Multi-environment deployments (dev / test / prod)  
✔ Remote state management using S3 backend with locking  
✔ Secure secrets handling with GitHub Environments  
✔ Terraform linting and validation  
✔ Security scanning using Trivy  
✔ Manual approval gates for production  
✔ Automated infrastructure destroy workflow  

---

## 🧱 Architecture Overview

The pipeline manages a **highly available AWS infrastructure**, including:

- VPC with public & private subnets  
- Application Load Balancer  
- Auto Scaling Groups  
- NAT Gateways  
- Multi-AZ deployment  

All infrastructure changes are driven by **GitHub pull requests and merges**.

---

## 🔄 CI/CD Workflow Design

### 1️⃣ Pull Request (Plan Phase)

Triggered on PR creation:

- Terraform init
- `terraform validate`
- TFLint checks
- Trivy security scan
- `terraform plan`
- Plan artifact uploaded for review

👉 **No resources are modified at this stage**

---

### 2️⃣ Merge to Branch (Apply Phase)

Triggered on merge:

- Workspace selection based on branch
- Terraform apply using stored plan
- Manual approval required for production
- Infrastructure updated automatically

Branch → Workspace mapping:
- `dev` → dev workspace
- `test` → test workspace
- `main` → prod workspace

---

### 3️⃣ Destroy Pipeline (Controlled Teardown)

- Separate GitHub Action workflow
- Manual environment selection
- Approval required
- Full infrastructure cleanup

Used for:
- Cost control
- Testing lifecycle completeness
- Safe teardown

---

## 🔐 Security & Governance

- AWS credentials stored as **GitHub Secrets**
- Environment-level protection rules
- Production requires **manual approval**
- Trivy scans detect:
  - Open security groups
  - Public IP exposure
  - Missing HTTPS
  - Weak networking patterns

Security issues fail the pipeline early.

---

## 🧪 Validation & Testing

✔ Terraform plan/apply verified via pipeline logs  
✔ Auto Scaling changes reflected in AWS console  
✔ Load balancer DNS validated post-deployment  
✔ Trivy reports surface real misconfigurations  
✔ Destroy workflow confirms full lifecycle control  

---

## 🛠 Tools Used

- Terraform
- GitHub Actions
- AWS S3 (remote state)
- DynamoDB (state locking)
- TFLint
- Trivy

---

## 📌 Key Takeaways

- Manual Terraform CLI does not scale
- CI/CD pipelines are mandatory in real environments
- Plan and apply must be separated
- Security scanning belongs in CI, not after deployment
- Production changes must be gated
- Terraform lifecycle must include **destroy**

---

## 🏁 Conclusion

Day 27 demonstrates **end-to-end Terraform automation using GitHub Actions**, covering planning, validation, security scanning, deployment, and destruction across multiple environments.

This is **real DevOps Terraform**, not demos.

---

> ⚠️ Always destroy resources after testing to avoid unnecessary AWS costs.
