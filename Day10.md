# 🚀 Terraform Day 10: Expressions — Conditional, Dynamic & Splat

Day 10 of the **30 Days of AWS Terraform** challenge focuses on **Terraform Expressions**, a powerful feature that helps write cleaner, smarter, and more scalable infrastructure code.

Expressions allow Terraform to make decisions, iterate dynamically, and extract values efficiently — reducing repetition and hardcoding.

---

## 🎯 Objectives

- Understand Terraform expressions and why they matter
- Use conditional expressions for environment-based logic
- Generate nested blocks dynamically using dynamic blocks
- Extract multiple resource attributes using splat expressions
- Write reusable and scalable Terraform configurations

---

## 🔹 What Are Terraform Expressions?

Terraform expressions are inline constructs that evaluate values dynamically at runtime.

They help:
- Avoid repeated code
- Adapt infrastructure based on inputs
- Work with complex variables
- Simplify real-world Terraform configurations

---

# 🔑 Expressions Covered

---

## 1️⃣ Conditional Expressions

Conditional expressions work like a one-line if–else.

### Syntax
```hcl
condition ? true_value : false_value
Example
hcl
Copy code
instance_type = var.env == "dev" ? "t2.micro" : "t3.medium"
Use Case
Environment-based instance sizing

Feature toggles

Cost optimization

2️⃣ Dynamic Blocks
Dynamic blocks generate nested blocks dynamically based on variable input.

Problem
Writing multiple repeated blocks manually:

hcl
Copy code
ingress { ... }
ingress { ... }
Solution: Dynamic Block
hcl
Copy code
dynamic "ingress" {
  for_each = var.ingress_rules
  content {
    from_port   = ingress.value.from_port
    to_port     = ingress.value.to_port
    protocol    = ingress.value.protocol
    cidr_blocks = ingress.value.cidr_blocks
    description = ingress.value.description
  }
}
Variable Example (List of Objects)
hcl
Copy code
variable "ingress_rules" {
  type = list(object({
    from_port   = number
    to_port     = number
    protocol    = string
    cidr_blocks = list(string)
    description = string
  }))
}
Best Used For
Security group rules

Load balancer listeners

Policy statements

Nested configurations

3️⃣ Splat Expressions
Splat expressions extract attributes from multiple resource instances created using count or for_each.

Example Resource
hcl
Copy code
resource "aws_instance" "web" {
  count = 2
}
Splat Expression
hcl
Copy code
aws_instance.web[*].id
Output Example
hcl
Copy code
output "instance_ids" {
  value = aws_instance.web[*].id
}
Terraform automatically collects all instance IDs into a list.

🧠 Debugging & Best Practices
Always refer to official Terraform documentation

Validate complex variables carefully

Test expressions incrementally

Avoid blindly trusting AI autocomplete

Read Terraform error messages closely

🛠 Commands Used
sh
Copy code
terraform init
terraform plan
terraform apply
terraform destroy
⭐ Key Takeaways
Conditional expressions reduce environment-based duplication

Dynamic blocks eliminate repetitive nested blocks

Splat expressions simplify aggregation of values

Expressions make Terraform flexible and scalable

Complex variables + expressions = powerful IaC

Essential for production-grade Terraform

❓ FAQs
Are expressions different from functions?
Yes. Expressions are inline logic; functions are predefined helpers.

When should I use dynamic blocks?
When you need multiple nested blocks based on variable input.

Do splat expressions work with for_each?
Yes, they work with both count and for_each.

Can expressions be combined together?
Yes. Conditional, dynamic, and splat expressions work seamlessly together.

🏁 Conclusion
Day 10 marks a major step toward writing intelligent Terraform code.

Terraform expressions help:

Reduce hardcoding

Improve readability

Enable dynamic behavior

Build scalable AWS infrastructure

These concepts are critical for real-world Terraform projects.

Next: Day 11 — Advanced Iteration & Terraform Internals 🚀
