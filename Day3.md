# 🚀 Terraform Day 3: Creating an AWS S3 Bucket

> “Code that builds cloud resources changes everything.”

Day 3 was all about moving from learning concepts to **building real infrastructure**.

Today’s task:  
✅ Provision an AWS S3 bucket using Terraform  
✅ Modify the resource  
✅ Destroy it safely  

This was my first complete Terraform lifecycle execution.

---

## 🎯 Objective

The purpose of Day 3:

- Learn practical resource provisioning
- Understand the Terraform workflow
- Observe how Terraform tracks state
- Practice update and destroy operations
- Build confidence using real AWS services

---

## 🛠 Terraform Configuration

Create a `main.tf` file:

### Provider
```hcl
provider "aws" {
  region = "us-east-1"
}
Resource
hcl
Copy code
resource "aws_s3_bucket" "demo" {
  bucket = "my-terraform-s3-unique-1234"

  tags = {
    Name = "Terraform Demo Bucket"
  }
}
⚠ S3 bucket names must be globally unique.

⚙ Terraform Commands Used
Initialize project
csharp
Copy code
terraform init
Preview changes
nginx
Copy code
terraform plan
Create infrastructure
nginx
Copy code
terraform apply
Destroy resources
nginx
Copy code
terraform destroy
🔁 Updating Resource
After deployment, tags were updated inside main.tf.

Terraform detected the change:

✅ terraform plan showed updates
✅ terraform apply modified existing resources

No recreation — only update.

🔐 AWS Authentication
Before using Terraform:

nginx
Copy code
aws configure
Terraform reads credentials from:

AWS CLI

Environment variables

IAM roles

🧠 Key Learnings
Terraform manages full resource life cycle

State file tracks infrastructure reality

terraform plan avoids surprises

Infrastructure updates are safe

Destroy cleans everything

Docs matter more than autocomplete

🏁 Conclusion
Day 3 made Terraform real.

Resources were:

Created

Modified

Destroyed

…all through code.

Tomorrow: More automation 🔥

<img width="1844" height="871" alt="Screenshot from 2025-12-10 05-23-27" src="https://github.com/user-attachments/assets/e252812e-bb2b-4776-ae7b-89351685ad62" />
