# 🚀 Day 21 – AWS Policy & Governance Automation with Terraform

This project demonstrates how to **enforce security policies and governance controls on AWS using Terraform**.  
It focuses on **preventive controls (IAM policies)** and **detective controls (AWS Config)** — exactly how real organizations manage compliance at scale.

---

## 🎯 Objective

The goal of Day 21 is to:

- Enforce **security and compliance rules** using IAM policies
- Enable **continuous compliance monitoring** using AWS Config
- Store audit logs securely in S3
- Detect and troubleshoot non-compliant resources
- Understand real-world policy evaluation behavior in AWS

---

## 🧠 Core Concepts Covered

### 🔐 Policy vs Governance

| Concept | Tool | Purpose |
|------|------|--------|
| **Policy** | IAM | Prevent actions before they occur |
| **Governance** | AWS Config | Detect violations after resources exist |

Terraform is used to automate **both**.

---

## 🏗️ Architecture Overview

### Components Created with Terraform:

- Encrypted & versioned **S3 audit bucket**
- **IAM policies** enforcing:
  - MFA for delete actions
  - Encryption-in-transit
  - Mandatory resource tagging
- **AWS Config Recorder**
- **AWS Config Delivery Channel**
- Multiple **AWS Config managed rules**

---

## 📁 Project Structure

```text
day21-policy-governance/
├── main.tf
├── providers.tf
├── variables.tf
├── iam-policies.tf
├── config-rules.tf
├── s3-audit-bucket.tf
├── outputs.tf
└── README.md
Terraform automatically loads all .tf files in the directory.

🔒 IAM Policies Implemented
The following policies are enforced:

❌ Block delete actions without MFA

❌ Block S3 uploads without HTTPS

❌ Block EC2 creation without required tags

These policies are:

Defined as JSON

Managed via Terraform

Attached to users and roles programmatically

📊 AWS Config Rules Enabled
AWS Config is used for continuous compliance checks:

S3 public access blocked

S3 & EBS encryption enforced

Required resource tags validated

Root account MFA monitoring

Encrypted EBS volumes check

AWS Config detects and reports NON-COMPLIANT resources automatically.

🧪 Testing & Validation
This project includes real-world testing:

Manual creation of non-compliant resources

AWS Config detecting violations

IAM policy behavior verification

Troubleshooting unexpected permission outcomes

This reflects actual enterprise AWS behavior, not idealized demos.

⚠️ Important Real-World Insight
IAM policies may not always behave as expected due to:

Incorrect attachments

Overlapping managed policies

Permission evaluation order

Always test policy behavior — never assume.

🧹 Cleanup
After testing, destroy all resources to avoid costs:

bash
Copy code
terraform destroy
⚠️ Ensure S3 buckets are emptied before destroy.

✅ Key Takeaways
Terraform can automate security and governance

IAM = preventive controls

AWS Config = detective controls

Secure audit logging is mandatory

Policy testing is as important as policy creation

🏁 Conclusion
Day 21 moves Terraform into enterprise governance territory.

By combining IAM policies, AWS Config, and secure audit storage, this project demonstrates how Infrastructure as Code can:

Enforce rules

Detect violations

Enable audits

Scale governance across AWS accounts

This is real-world Terraform usage.
