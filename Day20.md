# 🚀 Terraform Day 20: Production Infrastructure with Custom Terraform Modules

Day 20 focuses on building **production-grade AWS infrastructure** using **custom Terraform modules**.  
This day explains how real-world Terraform projects are structured using a **root module** that orchestrates multiple **reusable custom modules**.

The concepts demonstrated here are essential for **scalable, maintainable, enterprise-ready infrastructure**, including Kubernetes (EKS) platforms.

---

## 🎯 Objective

The objectives of Day 20 are to:

- Understand Terraform **modules deeply**
- Learn the difference between **public, partner, and custom modules**
- Design infrastructure using **root module + custom modules**
- Implement reusable components like:
  - VPC
  - EKS
  - IAM
  - Secrets Manager
- Learn **module communication using inputs and outputs**

This reflects how Terraform is used in **real production environments**.

---

## 🧠 What Is a Terraform Module?

A Terraform module is a **self-contained, reusable unit of Terraform code**.

A typical module contains:
- `main.tf` – resource definitions
- `variables.tf` – input variables
- `outputs.tf` – exposed values

Every directory in Terraform is technically a module.

---

## 📦 Types of Terraform Modules

### 1️⃣ Public Modules
- Available from the Terraform Registry
- Maintained by providers or the community
- Limited customization

### 2️⃣ Partner Modules
- Co-managed by HashiCorp and partners
- Verified and production-ready
- Still externally controlled

### 3️⃣ Custom Modules (Used in Day 20)
- Created and maintained by your organization
- Full control over:
  - Code
  - Security
  - Versioning
  - Updates

Custom modules are **mandatory in enterprise Terraform setups**.

---

## 🏗 Root Module vs Custom Modules

### Root Module
- Main project directory
- Entry point for `terraform init` and `terraform apply`
- Orchestrates all infrastructure
- Passes variables to modules and consumes outputs

### Custom Modules
- Stored as subdirectories inside the root module
- Each module has **one responsibility**
- No direct communication with other modules

The root module acts as the **central coordinator**.

---

## 📂 Project Structure

```text
terraform-project/
├── main.tf
├── variables.tf
├── outputs.tf
├── modules/
│   ├── vpc/
│   │   ├── main.tf
│   │   ├── variables.tf
│   │   └── outputs.tf
│   ├── eks/
│   │   ├── main.tf
│   │   ├── variables.tf
│   │   └── outputs.tf
│   ├── iam/
│   └── secrets/
Each module encapsulates a single infrastructure domain.

🔁 How Modules Communicate
Custom modules do not communicate directly.

Communication flow:

Root module calls a module

Inputs are passed as variables

Module creates resources

Module exposes outputs

Root module consumes outputs

Outputs are passed to other modules as inputs

This ensures:

Loose coupling

Clear dependency flow

Easy scaling

🔗 Example: VPC → EKS Dependency
VPC module outputs vpc_id

Root module captures vpc_id

Root module passes vpc_id to EKS module

EKS cluster is created inside the correct VPC

This pattern is standard in production.

📥 Variables and Outputs
Variables
Define what a module expects

Passed from the root module

Make modules reusable and flexible

Outputs
Define what a module exposes

Used by the root module

Enable coordination between modules

Modules behave like functions:

Inputs → Processing → Outputs

🧠 Why Custom Modules Matter in Production
Without modules:

Code becomes monolithic

Copy-paste increases bugs

Teams struggle to collaborate

With custom modules:

Infrastructure is reusable

Standards are enforced

Versioning is possible

Reviews are easier

Changes are safer

This is how large teams manage Terraform.

🧠 Key Learnings
Modules are the foundation of scalable Terraform

Root module orchestrates infrastructure

Custom modules provide full control

Inputs and outputs connect modules

Modules never talk directly to each other

Modular design is essential for Kubernetes-scale systems

🏁 Conclusion
Day 20 moves Terraform from simple configurations to architected systems.

By using custom modules:

Infrastructure becomes reusable

Code stays maintainable

Systems scale cleanly

This modular approach is the backbone of production-grade Terraform.
