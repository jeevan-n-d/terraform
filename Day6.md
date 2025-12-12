# 🚀 Terraform Day 6: Project File Structure & Best Practices

Day 6 focuses on moving from a beginner-style single `main.tf` file to a clean, professional, multi-file Terraform project structure.  
A well-organized Terraform project improves readability, maintainability, scalability, and security.

---

## 🎯 Objective

- Understand recommended Terraform folder/file structure  
- Split configuration into logical `.tf` files  
- Learn why Terraform state files must NOT be version-controlled  
- Prepare for future topics like modules and multi-environment setups  

---

# 📁 Recommended Terraform File Structure

Terraform automatically loads **all `.tf` files** in the directory, regardless of file names.  
A clean structure looks like this:

project/
├── main.tf → resources
├── variables.tf → input variable definitions
├── outputs.tf → output values
├── providers.tf → provider configuration (AWS, etc.)
├── backend.tf → remote backend configuration (S3, etc.)
├── locals.tf → computed/reusable expressions
├── terraform.tfvars → default variable values (optional)
└── .gitignore → excludes sensitive & generated files

---

# 🛠 Splitting Terraform Files (Hands-On)

### ✔ `backend.tf`
Stores remote backend configuration (state file location).

```hcl
terraform {
  backend "s3" {
    bucket = "my-backend-bucket"
    key    = "dev/terraform.tfstate"
    region = "us-east-1"
  }
}
}
✔ providers.tf
Defines provider(s):

hcl
Copy code
provider "aws" {
  region = var.region
}
✔ variables.tf
All input variable declarations.

✔ locals.tf
Reusable computed expressions.

✔ outputs.tf
Declare resource outputs.

✔ main.tf
Contains resources only.

This separation improves clarity and makes debugging easier.

🔐 Creating a Proper .gitignore

State files and Terraform metadata must never be committed to GitHub.

Recommended .gitignore entries:

.terraform/
terraform.tfstate
terraform.tfstate.backup
crash.log
*.tfvars
*.tfvars.json

Why exclude these?

State files contain sensitive data (ARNs, IDs, credentials metadata)

.terraform/ holds local provider plugins

Backups/logs clutter version control

.tfvars may contain environment secrets

🧱 Advanced Structures (Introduced Briefly)

Day 6 also introduces two scalable approaches used in real projects:

1. Environment-based folders
environments/
 ├── dev/
 ├── stage/
 └── prod/

2. Module-based structure
modules/
 ├── networking/
 ├── compute/
 ├── security/


These will be covered in later videos; beginners should first master basic file separation.

⭐ Key Points

Splitting Terraform files improves organization and readability

Terraform loads all .tf files automatically—filename is only for human clarity

Always use .gitignore to exclude state files and sensitive artifacts

Multi-env and module structures help scale large projects

Organizing Terraform code is essential for collaboration and long-term maintenance

❓ FAQ
Why not keep everything in one main.tf?

It becomes unmanageable as the project grows.

Do file names matter to Terraform?

No. Terraform reads all .tf files automatically.

Which files must NOT go to GitHub?

terraform.tfstate

.terraform/ folder

backups, logs, .tfvars

How do teams handle dev/stage/prod?

Either separate folders or multiple .tfvars files.

🏁 Conclusion

Day 6 introduces the foundation of clean Terraform project organization.
By structuring files properly and securing sensitive data, your Terraform project becomes easier to maintain, safer to share, and ready for real-world scaling.

Next: Day 7 — Type Constraints (validating variable inputs). 🚀
