# 🚀 Terraform Day 7: Variable Type Constraints

Day 7 of the **30 Days of AWS Terraform Challenge** focuses on one of the most important concepts in Terraform:  
**Type Constraints** — defining what kind of data your variables are allowed to accept.

This ensures your Terraform code is safe, predictable, and free from incorrect inputs.  
Primitive and complex data types were demonstrated with real AWS examples such as EC2 instances and security groups.

---

## 🎯 Objectives

- Understand Terraform variable types  
- Learn to enforce data correctness using type constraints  
- Work with primitive and complex types  
- Apply variable types in realistic AWS resource configurations  
- Avoid type mismatch errors using validation  

---

# 🔹 Primitive Types

Terraform supports three main primitive types:

### **1. number**
Used for values such as ports, counts, or sizes.

```hcl
variable "instance_count" {
  type = number
}
2. string
Used for region, environment name, AMI IDs, etc.

hcl
Copy code
variable "region" {
  type = string
}
3. bool
Used for toggles such as enabling EC2 monitoring or associating a public IP.

hcl
Copy code
variable "enable_monitoring" {
  type = bool
}
Example usage inside AWS EC2:

hcl
Copy code
monitoring                    = var.enable_monitoring
associate_public_ip_address   = var.attach_ip
🔸 Complex Types
Terraform includes advanced data types for structured and flexible inputs.

1. list(type)
Ordered collection

Allows duplicates

Indexable

Example: multiple CIDR blocks for security groups

hcl
Copy code
variable "allowed_cidrs" {
  type = list(string)
}
Use:

hcl
Copy code
cidr_blocks = var.allowed_cidrs
Indexing:

hcl
Copy code
var.allowed_cidrs[0]
2. set(type)
Unordered

No duplicates

Cannot access by index directly

To index a set:

hcl
Copy code
tolist(var.allowed_ips)[0]
Difference:

Feature	List	Set
Ordered	Yes	No
Duplicates	Allowed	Removed
Indexing	Yes	No (requires conversion)

3. map(type)
Key-value pairs with the same value type.

Commonly used for tags.

hcl
Copy code
variable "tags" {
  type = map(string)
}
Usage:

hcl
Copy code
tags = var.tags
4. tuple([...])
Fixed-length

Ordered

Supports mixed types

Example:

hcl
Copy code
variable "server_config" {
  type = tuple([string, number, bool])
}
Access:

hcl
Copy code
var.server_config[1]
5. object({...})
More flexible than maps — each key can have its own type.

hcl
Copy code
variable "metadata" {
  type = object({
    name    = string
    version = number
    active  = bool
  })
}
Use:

hcl
Copy code
var.metadata.name
🔸 Special Types
any
Accepts any value — not recommended for production.

null
Represents "no value" and often triggers defaults.

🛠 Real-World AWS Examples Covered
✔ EC2 instance creation using number, string, and boolean types
✔ Security group ingress rules using list and set
✔ Tag management using a map
✔ Metadata grouping using object
✔ Mixed-type collections using tuple
The session demonstrated how type mismatch errors appear and how Terraform resolves them with proper constraints.

⭐ Key Takeaways
Always define variable types — avoid using untyped variables

Primitive types are simple but essential for correctness

Lists and sets behave differently; choose based on ordering & duplicates

Maps are ideal for repeated key-value inputs like tags

Tuples enforce strict typed ordering

Objects make complex configurations readable

Converting sets to lists helps when indexing

Validation happens early during terraform plan

❓ Frequently Asked Questions
Why use type constraints?
To validate inputs early and prevent runtime errors.

Difference between list and set?
List: ordered, allows duplicates

Set: unordered, unique values

When to use tuple vs object?
Tuple: when order and type of each element matter

Object: when keys and their types matter

Can a set be indexed?
No — unless converted using tolist().

What happens if you pass the wrong type?
Terraform stops with a type mismatch error during plan or apply.

🏁 Conclusion
Day 7 provided a comprehensive understanding of Terraform’s type system.
By mastering primitive and complex types — and applying them to AWS examples — you build safer, clearer, and more scalable Terraform configurations.

These concepts become essential as we move toward modules, dynamic expressions, loops, and advanced infrastructure design.

Next: Day 8 — Terraform Functions & Expressions 🚀
