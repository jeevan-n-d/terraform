🚀 Terraform Day 5: Variables — Clean, Reusable, and Scalable Infrastructure Code

Day 5 of the 30 Days AWS Terraform Challenge focused on Variables — one of the most essential concepts for writing clean, modular, and production-ready Terraform configurations.

This README summarizes everything learned:

What variables are

Why they matter

Types of variables

How Terraform decides which value to use

Real practical usage and best practices

🎯 Objective of Day 5

The goal of this lesson:

Avoid hardcoding values in Terraform

Introduce input variables, local values, and outputs

Learn Terraform variable types

Understand variable precedence

Practice environment switching using variables

Improve maintainability and reduce errors

Variables help centralize configuration values and make Terraform code flexible for dev, stage, prod, and other environments.

🧩 1. What Are Variables in Terraform?

Variables enable Terraform scripts to become:

Reusable

Configurable

Cleaner

Less error-prone

Instead of hardcoding values (region, names, CIDR blocks, tags), variables let you reference values dynamically using:

var.<variable_name>

🔸 2. Input Variables

Used to pass values into Terraform from the user or environment.

Declaration (variables.tf)
variable "env" {
  type    = string
  default = "dev"
}

Usage
tags = {
  Environment = var.env
}


Input variables replace repeated values and allow environment switching.

🔹 3. Local Values (locals)

Locals are computed values used inside Terraform to simplify expressions.

Example:
locals {
  bucket_name = "${var.env}-myapp-bucket"
}

Usage:
bucket = local.bucket_name


Locals help maintain consistent naming conventions and reduce duplication.

🔸 4. Output Variables

Outputs display useful resource information after Terraform deploys.

Example:
output "vpc_id" {
  value = aws_vpc.main.id
}


Run:

terraform output


Outputs are helpful for:

Debugging

Passing values between modules

CI/CD pipelines

🧠 5. Variable Types in Terraform

Terraform supports many types:

Primitive Types

string

number

bool

Complex Types

list(string)

set(string)

map(string)

object({})

tuple([])

Special

any

null

Example:

variable "allowed_ips" {
  type = list(string)
}


Types improve validation and prevent incorrect input.

🔀 6. Variable Precedence (Very Important)

Terraform decides variable values based on the following order (highest → lowest):

1️⃣ CLI override (Highest precedence)
terraform apply -var="env=stage"

2️⃣ tfvars file

Example: dev.tfvars

env = "dev"


Run:

terraform apply -var-file="dev.tfvars"

3️⃣ Environment variable
export TF_VAR_env="prod"

4️⃣ Default value inside variables.tf (Lowest)
default = "dev"


Knowing precedence is essential for environment-specific configuration.

🧪 7. Hands-On Commands Used Today
1. Initialize Terraform
terraform init

2. Preview infrastructure changes
terraform plan

3. Apply infrastructure
terraform apply

4. Show output values
terraform output

5. Destroy resources
terraform destroy

⚠️ 8. Troubleshooting Learnings

Use official AWS provider documentation for correct resource names

AMI IDs must be valid for chosen region

Fix permission issues using IAM policies

Avoid copying outdated code from other sources

⭐ Key Takeaways from Day 5

Variables eliminate repetition

Locals simplify expressions

Outputs expose important resource values

Precedence order makes Terraform flexible

Use tfvars files for real environments

Never hardcode sensitive data

Use variables for all environment-specific settings

❓ FAQ
Q: Why avoid hardcoding values?

To prevent mistakes and support multiple environments easily.

Q: Difference between input variables and locals?

Input variables = user-defined external inputs

Locals = internal computed values

Q: How do I switch environments?

Use:

terraform apply -var-file=dev.tfvars

Q: Why use output variables?

To display resource attributes like VPC ID, subnet IDs, instance IPs, etc.

🏁 Conclusion

Day 5 introduces one of the most important skills in Terraform: managing values properly.

By using:

Input variables

Local values

Output variables

tfvars files

Precedence rules

…you transform Terraform from a basic script into professional, scalable Infrastructure as Code.

Variables are the foundation of everything you will build next.


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
