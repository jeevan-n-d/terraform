# 🚀 Terraform Day 13: Data Sources — Using Existing AWS Infrastructure

Day 13 of the **30 Days of AWS Terraform** challenge focuses on **Terraform Data Sources** — a critical concept for building **dynamic, reusable, and enterprise-ready infrastructure**.

Instead of hardcoding values or recreating resources, data sources allow Terraform to **query and reuse existing AWS infrastructure** safely.

---

## 🎯 Objectives

- Understand what Terraform data sources are
- Learn why hardcoding AWS values is dangerous
- Fetch existing AWS resources dynamically
- Use data sources for AMIs, VPCs, and subnets
- Provision EC2 instances inside existing infrastructure

---

## 🔹 What Is a Terraform Data Source?

A **data source** allows Terraform to **read information** about existing resources.

- ❌ Does NOT create resources
- ✅ Only fetches existing data
- ✅ Used inside Terraform configurations like variables

Data sources are essential in:
- Shared AWS accounts
- Enterprise environments
- CI/CD pipelines
- Automation workflows

---

## 🚫 Why Hardcoding Is a Bad Practice

Hardcoding values like:
- AMI IDs
- VPC IDs
- Subnet IDs

Causes:
- Broken automation
- Manual updates
- Security risks
- Poor scalability

Terraform data sources solve this problem.

---

## 1️⃣ Fetching Latest Amazon Linux AMI (Best Practice)

AMI IDs change frequently and are region-specific.

### Data Source for AMI
```hcl
data "aws_ami" "amazon_linux" {
  most_recent = true
  owners      = ["amazon"]

  filter {
    name   = "name"
    values = ["amzn2-ami-hvm-*-x86_64-gp2"]
  }

  filter {
    name   = "virtualization-type"
    values = ["hvm"]
  }
}
✔ Always fetches the latest secure AMI
✔ No hardcoded values

2️⃣ Using Data Source for Existing VPC
In real environments, VPCs are often pre-created.

hcl
Copy code
data "aws_vpc" "main" {
  filter {
    name   = "tag:Name"
    values = ["shared-vpc"]
  }
}
✔ Selects VPC by tag
✔ Scales well in large AWS accounts

3️⃣ Using Data Source for Existing Subnet
Subnets are selected using both tags and VPC ID.

hcl
Copy code
data "aws_subnet" "public" {
  filter {
    name   = "tag:Name"
    values = ["public-subnet-1"]
  }

  filter {
    name   = "vpc-id"
    values = [data.aws_vpc.main.id]
  }
}
✔ Prevents wrong subnet selection
✔ Ensures correct network placement

4️⃣ Using Data Sources in Resource Definitions
Fetched data can be referenced directly inside resources.

hcl
Copy code
resource "aws_instance" "web" {
  ami           = data.aws_ami.amazon_linux.id
  instance_type = "t2.micro"
  subnet_id     = data.aws_subnet.public.id
}
Terraform:

Reads existing infra

Creates only required resources

Avoids duplication and mistakes

🔁 Terraform Workflow Used
sh
Copy code
terraform init
terraform plan
terraform apply
terraform destroy
✔ Always review plan
✔ Destroy test resources to avoid costs

⭐ Key Takeaways
Data sources read existing AWS infrastructure

Never hardcode AMI, VPC, or subnet IDs

Filters are essential for accurate selection

Tag-based filtering scales best

Latest AMIs improve security

Data sources enable reusable Terraform code

Mandatory for real-world automation

❓ FAQs
What is a Terraform data source?
A read-only reference to existing infrastructure.

Can data sources create resources?
No. They only fetch information.

Why should I avoid hardcoding AMI IDs?
AMI IDs change frequently and break automation.

Can I reference existing VPCs and subnets?
Yes, using data sources with filters.

What happens if I don’t destroy test resources?
You may incur ongoing AWS charges.

🏁 Conclusion
Day 13 introduces one of the most important Terraform concepts for production use.

By using data sources, you can:

Integrate with existing AWS environments

Build scalable, reusable Terraform code

Eliminate manual steps and hardcoding

Follow enterprise-grade best practices
