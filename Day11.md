# 🚀 Terraform Day 11: Built-in Functions — Writing Smarter IaC

Day 11 of the **30 Days of AWS Terraform** challenge focuses on **Terraform Built-in Functions**.  
Functions bring programming-style logic into Terraform, helping reduce repetition, enforce standards, and build dynamic infrastructure configurations.

Terraform does **not support custom functions**, so understanding and mastering **built-in functions** is essential for real-world usage.

---

## 🎯 Objectives

- Understand what Terraform functions are
- Learn why functions matter in IaC
- Explore major function categories
- Practice functions using Terraform console
- Apply functions in real AWS Terraform configurations

---

## 🔹 What Are Terraform Functions?

Functions are reusable logic blocks that transform input values.

Terraform:
- ❌ Does not allow user-defined functions
- ✅ Provides many **built-in functions**

Functions help:
- Manipulate strings and numbers
- Work with lists, maps, and sets
- Convert data types
- Handle dates and timestamps
- Select values dynamically

---

## 🧪 Terraform Console

Terraform console allows testing functions interactively.

```sh
terraform console
Example:

hcl
Copy code
upper("terraform")
length(["a", "b", "c"])
This is the fastest way to learn and debug functions.

🔑 Function Categories Covered
1️⃣ String Functions
Used for formatting names, tags, and user input.

Examples:

hcl
Copy code
upper("aws")          # AWS
lower("AWS")          # aws
trim(" aws ")         # aws
replace("my app", " ", "-")
substr("terraform", 0, 4)
Used heavily to meet AWS naming constraints.

2️⃣ Numeric Functions
Used for calculations and comparisons.

hcl
Copy code
max(3, 7, 2)
min(5, 1, 9)
abs(-10)
Helpful for limits, validations, and counts.

3️⃣ Collection Functions (List & Map)
List Functions
hcl
Copy code
length(var.subnets)
concat(list1, list2)
Map Functions
hcl
Copy code
merge(map1, map2)
Example: Merging Tags
hcl
Copy code
locals {
  common_tags = {
    Project = "Terraform"
  }

  env_tags = {
    Environment = var.env
  }

  final_tags = merge(local.common_tags, local.env_tags)
}
4️⃣ Type Conversion Functions
Terraform is strictly typed.

Examples:

hcl
Copy code
toset(["a", "a", "b"])   # removes duplicates
tostring(123)
tonumber("42")
Used to fix type mismatches and enforce correctness.

5️⃣ Date & Time Functions
Used for logging, naming, and auditing.

hcl
Copy code
timestamp()
formatdate("YYYY-MM-DD", timestamp())
6️⃣ Sanitizing AWS Resource Names (Real Use Case)
AWS S3 bucket rules:

Lowercase only

3–63 characters

No spaces

Solution:
hcl
Copy code
locals {
  bucket_name = substr(
    replace(lower(var.bucket_name), " ", "-"),
    0,
    63
  )
}
Prevents AWS API and Terraform errors.

7️⃣ Split, For-Expressions & Interpolation
Split
hcl
Copy code
split(",", "dev,stage,prod")
For-Expression
hcl
Copy code
[for env in var.envs : upper(env)]
Interpolation
hcl
Copy code
"${var.project}-${var.env}"
Used for dynamic and scalable configurations.

8️⃣ Lookup Function
Selects values from a map with a default fallback.

hcl
Copy code
lookup(var.instance_type_map, var.env, "t2.micro")
Ideal for:

Environment-based configuration

Avoiding long conditional logic

🛠 Commands Used
sh
Copy code
terraform init
terraform plan
terraform apply
terraform output
⭐ Key Takeaways
Terraform relies entirely on built-in functions

terraform console is the best learning tool

String functions enforce AWS naming rules

Collection functions simplify tags and configs

Type conversions prevent runtime errors

lookup enables clean multi-environment logic

Functions make Terraform dynamic and maintainable

❓ FAQs
Can I create custom functions in Terraform?
No. Only built-in functions are supported.

How do I test functions quickly?
Use terraform console.

What is the difference between concat and merge?
concat joins lists; merge combines maps.

Why are functions important in AWS Terraform?
AWS enforces strict naming and validation rules.

🏁 Conclusion
Day 11 introduces Terraform functions as a core building block for professional infrastructure-as-code.

By mastering functions, you can:

Write cleaner Terraform code

Reduce errors

Improve reusability

Build environment-aware infrastructure
