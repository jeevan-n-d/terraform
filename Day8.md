# 🚀 Terraform Day 8: Meta Arguments (depends_on, count, for_each)

Day 8 of the **30 Days of AWS Terraform** challenge focuses on **Terraform Meta Arguments**.  
Meta arguments are special Terraform-built features that control **resource behavior**, not cloud provider configuration.

They are essential for:
- Managing resource dependencies
- Creating multiple resources dynamically
- Writing scalable and maintainable Terraform code

---

## 🎯 Objectives

- Understand what Terraform meta arguments are  
- Learn how `depends_on` controls resource order  
- Use `count` to replicate resources from lists  
- Use `for_each` to iterate over sets and maps  
- Understand differences between list, set, and map  
- Apply meta arguments in real AWS examples  

---

## 🔹 What Are Terraform Meta Arguments?

Meta arguments are **Terraform-native arguments** that can be used inside resource blocks.

They are **not provider-specific** and work the same for AWS, Azure, GCP, etc.

Key meta arguments covered today:
- `depends_on`
- `count`
- `for_each`

---

## 🔹 depends_on — Explicit Dependencies

Terraform usually detects dependencies automatically through references.

Example (implicit dependency):
```hcl
subnet_id = aws_subnet.main.id

Sometimes Terraform cannot infer the dependency.
In such cases, use depends_on.

Example:
resource "aws_s3_bucket" "bucket2" {
  bucket = "my-second-bucket"

  depends_on = [
    aws_s3_bucket.bucket1
  ]
}

Use depends_on when:

No direct reference exists

Resource order matters

External or indirect dependencies exist

⚠ Avoid overusing it — let Terraform infer dependencies when possible.

🔹 count — Iteration with Lists

count creates multiple copies of a resource.

Best suited for lists, because lists are ordered and indexable.

Variable:
variable "bucket_names" {
  type = list(string)
}

Resource:
resource "aws_s3_bucket" "buckets" {
  count  = length(var.bucket_names)
  bucket = var.bucket_names[count.index]
}

Key Points:

count.index starts at 0

Works well with lists

Not recommended for sets (unordered)

🔹 for_each — Iteration with Sets and Maps

for_each is safer and more flexible than count.

It works best with:

sets

maps

for_each with Set
variable "bucket_set" {
  type = set(string)
}

resource "aws_s3_bucket" "set_buckets" {
  for_each = var.bucket_set
  bucket   = each.value
}


Sets are unordered

each.key and each.value are the same

for_each with Map
variable "bucket_map" {
  type = map(string)
}

resource "aws_s3_bucket" "map_buckets" {
  for_each = var.bucket_map
  bucket   = each.value

  tags = {
    Name = each.key
  }
}


each.key → map key

each.value → map value

Maps are ideal for tagging and environment-based logic.

🔁 count vs for_each
Scenario	Recommended
Ordered list	count
Unordered collection	for_each
Maps	for_each
Stable resource identity	for_each
Simple duplication	count

Best practice: Prefer for_each for real-world projects.

⚠ Common Mistakes Covered

Using count with sets

Expecting index access on sets

Confusing Terraform resource names with actual AWS resource names

Overusing depends_on

Ignoring data structure differences

🛠 Commands Used
terraform init
terraform plan
terraform apply
terraform destroy

⭐ Key Takeaways

Meta arguments control Terraform behavior

depends_on enforces explicit resource order

count works best with lists

for_each is safer for sets and maps

Understanding data structures is critical

Meta arguments eliminate repetitive code

Enables scalable, predictable infrastructure
