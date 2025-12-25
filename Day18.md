# 🚀 Terraform Day 18: Serverless Automation with AWS Lambda Using Terraform

Day 18 focuses on building a **fully automated, event-driven serverless pipeline** using **AWS Lambda, S3, IAM, CloudWatch, and Terraform**.

This project demonstrates how to design, deploy, test, and troubleshoot a **real-world AWS Lambda workflow** using Infrastructure as Code (IaC).

---

## 🎯 Project Objective

The objective of this project is to:

- Deploy an **AWS Lambda function** using Terraform
- Trigger Lambda automatically on **S3 object uploads**
- Process images (resize / compress / format conversion)
- Store processed images in a **destination S3 bucket**
- Capture execution logs in **CloudWatch**
- Handle real-world dependency issues using **Docker-based builds**

All infrastructure is **fully automated and reproducible**.

---

## 🧠 Architecture Overview

The system is **event-driven**:

1. Image uploaded to **source S3 bucket**
2. S3 generates an `ObjectCreated` event
3. AWS Lambda is triggered automatically
4. Lambda processes the image using Python (Pillow)
5. Processed images are uploaded to **destination S3 bucket**
6. Logs are written to **CloudWatch Logs**

No servers to manage. No scaling logic required.

---

## 🏗 AWS Resources Managed by Terraform

Terraform provisions and manages:

- **S3 Source Bucket**
  - Private access
  - Versioning enabled
  - Encryption enabled
- **S3 Destination Bucket**
  - Stores processed images
- **IAM Role for Lambda**
  - Least-privilege permissions
- **Lambda Function**
  - Python runtime
  - Environment variables
- **Lambda Layer**
  - Pillow dependency
- **S3 Event Notification**
  - Triggers Lambda on upload
- **CloudWatch Log Group**

Everything is created using Terraform.

---

## 📂 Project Structure

```text
day18/
├── main.tf
├── variables.tf
├── outputs.tf
├── providers.tf
├── lambda/
│   └── handler.py
├── layer/
│   └── python/
├── scripts/
│   ├── deploy.sh
│   └── destroy.sh
🔐 IAM Security (Least Privilege)
The Lambda execution role allows only:

s3:GetObject on source bucket

s3:PutObject on destination bucket

CloudWatch log creation and write access

No extra permissions are granted.

🧩 Lambda Dependency Challenge
The Lambda function depends on Pillow for image processing.

Problem encountered:

Lambda runs on Amazon Linux

Local builds (Windows / macOS) produce incompatible binaries

Result:

ImportError: cannot import name '_imaging'

This is a common real-world issue.

🐳 Solving Dependency Issues with Docker
Solution:

Build Lambda dependencies inside a Docker container

Docker image matches AWS Lambda runtime

Pillow is compiled correctly for Lambda

Layer is packaged and deployed via Terraform

This ensures:

Runtime compatibility

Predictable builds

Zero import errors

⚙️ Deployment Steps
1️⃣ Build & Deploy Infrastructure
bash
Copy code
./scripts/deploy.sh
This script:

Builds Lambda layer

Initializes Terraform

Runs plan and apply

2️⃣ Test the Pipeline
Upload an image to the source bucket:

bash
Copy code
aws s3 cp sample.jpg s3://<source-bucket-name>/
Expected behavior:

Lambda is triggered

Image is processed

Output images appear in destination bucket

Logs visible in CloudWatch

🔍 Debugging & Monitoring
CloudWatch Logs show:

Execution duration

Memory usage

Errors (if any)

Useful for:

Debugging

Performance tuning

Cost analysis

🧹 Cleanup (Important)
To avoid AWS charges:

Empty both S3 buckets (including versions)

Destroy infrastructure:

bash
Copy code
./scripts/destroy.sh
or

bash
Copy code
terraform destroy
🧠 Key Learnings
Serverless architectures eliminate server management

Terraform can manage Lambda + S3 + IAM cleanly

Event-driven systems scale automatically

IAM least privilege is critical

CloudWatch logs are essential for debugging

Docker solves Lambda dependency compatibility issues

Automation ensures repeatability and reliability

🏁 Conclusion
This project demonstrates how Terraform enables production-grade serverless automation.

By combining:

Event-driven design

Infrastructure as Code

Secure IAM practices

Docker-based dependency management

you get a scalable, cost-efficient, and reliable AWS Lambda solution.
