# 🚀 Terraform Day 5: Variables — Clean, Reusable & Scalable Configurations

Day 5 of the 30 Days AWS Terraform Challenge focuses on **Variables** — the foundation of writing reusable, maintainable, and scalable Terraform code.

Variables help avoid hardcoding values, reduce repetition, minimize mistakes, and make environment switching effortless.

---

## 🎯 Objective

- Understand why variables are needed  
- Learn input variables, local values (locals), and outputs  
- Explore variable types and constraints  
- Use variable precedence to manage dev/stage/prod configs  
- Write cleaner, modular Terraform scripts  

---

# 🧩 What Are Variables?

Variables enable Terraform to accept dynamic values instead of hardcoding values inside configuration files.

They help with:

- Reusability  
- Maintenance  
- Environment separation  
- Reducing human errors  

Access variable value using:

var.<variable_name>

yaml
Copy code

---

## 🔸 Input Variables

Used to receive values from the user or environment.

**Declare:**
```hcl
variable "env" {
  type    = string
  default = "dev"
}
Use:

hcl
Copy code
tags = {
  Environment = var.env
}
🔹 Local Values (locals)
Locals simplify Terraform code by storing computed or reusable expressions.

Example:

hcl
Copy code
locals {
  bucket_name = "${var.env}-myapp-bucket"
}
Use:

hcl
Copy code
bucket = local.bucket_name
🔸 Output Variables
Outputs show key resource information after Terraform applies resources.

Example:

hcl
Copy code
output "vpc_id" {
  value = aws_vpc.main.id
}
Show outputs:

lua
Copy code
terraform output
🧠 Variable Types
Terraform supports:

string

number

bool

list()

set()

map()

object()

tuple()

any

Example:

hcl
Copy code
variable "allowed_ips" {
  type = list(string)
}
🔀 Variable Precedence (Highest → Lowest)
Terraform chooses variable values using this order:

1️⃣ CLI override
csharp
Copy code
terraform apply -var="env=stage"
2️⃣ .tfvars file
Example dev.tfvars:

hcl
Copy code
env = "dev"
Run:

csharp
Copy code
terraform apply -var-file="dev.tfvars"
3️⃣ Environment variable
arduino
Copy code
export TF_VAR_env="prod"
4️⃣ Default value inside variable block
hcl
Copy code
default = "dev"
🧪 Commands Used Today
csharp
Copy code
terraform init
terraform plan
terraform apply
terraform output
terraform destroy
⚠️ Troubleshooting Notes
Always verify AMI IDs and resource names using official provider docs

Fix permission issues via IAM roles/policies

Do not rely solely on autocomplete — check docs

Use tfvars files for real environment configs

⭐ Key Takeaways
Variables remove duplication

Locals simplify expressions

Outputs expose resource details

Precedence enables multi-environment workflows

Variables make Terraform professional and scalable

Never hardcode values in real projects

❓ FAQs
Why not hardcode values?
It causes repetition and increases risk of mistakes.

Difference between input variables and locals?
Input variables: provided externally

Locals: internal computed values

Best way to manage different environments?
Use .tfvars files like dev.tfvars, stage.tfvars, etc.

Why use output variables?
To display important resource attributes (IDs, IPs, ARNs).

🏁 Conclusion
Day 5 builds the foundation for writing clean, reusable, and environment-friendly Terraform code.

By mastering:

Input variables

Local values

Output variables

Variable precedence

…Terraform becomes far more powerful and maintainable.

Next up: More advanced features in Terraform variables & configurations 🚀

CODE :
terraform {
 required_providers {
   aws = {
    source = "hashicorp/aws"
    version = "~> 6.0"
   }
 } 
}



provider "aws" {
    region = "ap-south-1"
}

variable "environment" {

  default = "dev"
  
}

variable "my_name" {
  default = "my-tf-jeeva"
}

variable "regions" {
  default = "ap-south-1"
}


locals {
  env = var.environment
  bucket_name = "${var.my_name}-${var.environment}-bucket"
  vpc_name = "${var.my_name}-${var.environment}-VPC"
  ec2_name= "${var.my_name}-${var.environment}-ec2"
  tag="my-terraform"
}


resource "aws_s3_bucket" "first-bucket" {
  bucket = local.bucket_name

  tags={
    Name = local.bucket_name
    Environment = var.environment
  }
}


resource "aws_vpc" "sample"{
  cidr_block = "10.0.0.0/16"
  region = var.regions
  tags =  {
    Environment = var.environment
    Name = local.vpc_name
  }
}

resource "aws_instance" "example" {
  ami="ami-0dee22c13ea7a9a67"
  instance_type = "t2.micro"
  region = var.regions
  
    tags =  {
    Environment = "${var.environment}-ec2"
    Name = local.ec2_name
  }
}

output "vpc_id" {
  value=aws_vpc.sample.id
}

output "ec2_id" {
  value=aws_instance.example.id
}

output "s3_id" {
  value=aws_s3_bucket.first-bucket.id
}


output "bucket_name" {
  description = "Name of the S3 bucket"
  value       = aws_s3_bucket.first-bucket.bucket
}

output "bucket_arn" {
  description = "ARN of the S3 bucket"
  value       = aws_s3_bucket.first-bucket.arn
}


output "environment" {
  description = "Environment from input variable"
  value       = var.environment
}


output "tags" {
  description = "Tags from local variable"
  value       = local.tag
}
