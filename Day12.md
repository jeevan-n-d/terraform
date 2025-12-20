# 🚀 Terraform Day 12: Validation, Numeric, Time & File Functions

Day 12 of the **30 Days of AWS Terraform** challenge continues the deep dive into **Terraform built-in functions**, focusing on writing **safer, cleaner, and more resilient infrastructure code**.

This day covers validation, sensitive variables, type conversion, numeric calculations, timestamps, and file handling — all essential for production-grade Terraform.

---

## 🎯 Objectives

- Validate user inputs before resource creation
- Protect sensitive values from being exposed
- Remove duplicate values from collections
- Perform numeric calculations on lists
- Work with timestamps and formatted dates
- Read and decode external configuration files

---

## 🔁 Why This Matters

Without proper validation and data handling:
- Terraform fails late (during apply)
- Invalid inputs reach AWS APIs
- Duplicate or incorrect values cause errors
- Configurations become fragile and hard to manage

Day 12 focuses on **preventing these issues early**.

---

## 1️⃣ Variable Validation Functions

Validation rules are defined **inside variable blocks**.

### Length Validation
```hcl
variable "instance_type" {
  type    = string
  default = "t2.micro"

  validation {
    condition     = length(var.instance_type) >= 2 && length(var.instance_type) <= 20
    error_message = "Instance type length must be between 2 and 20 characters."
  }
}
Regex Validation
hcl
Copy code
validation {
  condition     = can(regex("^t(2|3).*", var.instance_type))
  error_message = "Only t2 or t3 instance types are allowed."
}
endswith Validation
hcl
Copy code
variable "backup_name" {
  type = string

  validation {
    condition     = endswith(var.backup_name, "-backup")
    error_message = "Backup name must end with -backup."
  }
}
✅ Validation prevents invalid infrastructure before apply.

🔐 Sensitive Variables
Hide sensitive values from logs and outputs.

hcl
Copy code
variable "db_password" {
  type      = string
  sensitive = true
}
⚠ Note:

Sensitive values are not encrypted

Values still exist in the Terraform state

Use AWS Secrets Manager / Vault for real security

2️⃣ Type Conversion & Deduplication
Lists allow duplicates; sets do not.

Example
hcl
Copy code
locals {
  all_locations    = concat(var.default_locations, var.user_locations)
  unique_locations = toset(local.all_locations)
}
✔ toset() removes duplicate values
✔ Useful for regions, CIDRs, names

3️⃣ Numeric Functions with Collections
Terraform numeric functions work on individual values, not lists.

Example Input
hcl
Copy code
variable "monthly_costs" {
  type    = list(number)
  default = [100, -50, 200]
}
Convert to Positive Values
hcl
Copy code
locals {
  positive_costs = [for c in var.monthly_costs : abs(c)]
}
Aggregations
hcl
Copy code
locals {
  max_cost = max(local.positive_costs...)
  min_cost = min(local.positive_costs...)
  total    = sum(local.positive_costs)
  average  = sum(local.positive_costs) / length(local.positive_costs)
}
📌 Spread operator (...) is required for max() and min().

4️⃣ Timestamp & Date Functions
Current UTC Timestamp
hcl
Copy code
timestamp()
⚠ Value is known after apply.

Date Formatting
hcl
Copy code
formatdate("YYYY-MM-DD", timestamp())
Practical Naming Example
hcl
Copy code
"${var.project}-backup-${formatdate("YYYYMMDD", timestamp())}"
Used for:

Backups

Logs

Audit-friendly naming

5️⃣ File Handling Functions
Terraform can safely read external files.

Check if File Exists
hcl
Copy code
fileexists("config.json")
Read & Decode JSON
hcl
Copy code
locals {
  config = fileexists("config.json") ? jsondecode(file("config.json")) : {}
}
Sample JSON
json
Copy code
{
  "db_name": "appdb",
  "port": 5432
}
Access values as:

hcl
Copy code
local.config.db_name
📂 Useful for external configs and automation.

🛠 Commands Used
sh
Copy code
terraform init
terraform plan
terraform apply
terraform output
⭐ Key Takeaways
Validation rules catch errors early

Regex and logical operators enable strong checks

Sensitive variables hide values but don’t encrypt

toset() removes duplicates from lists

Numeric functions require loops or spread operator

Timestamps are apply-time values

File functions enable external configuration

Functions make Terraform safer and production-ready

❓ FAQs
Where do validation rules go?
Inside variable blocks using validation.

Are sensitive variables encrypted?
No, they are only hidden in outputs.

How do I remove duplicates from a list?
Convert it to a set using toset().

Why do max/min need ...?
They expect individual values, not a list.

Can Terraform read JSON files?
Yes, using file() with jsondecode().

🏁 Conclusion
Day 12 strengthens Terraform fundamentals by adding:

Input safety

Data correctness

Time-based logic

External configuration handling

These concepts are essential for robust, scalable AWS infrastructure automation.
