🚀 Terraform Day 23: Production-Grade Monitoring & Observability on AWS (Serverless)

Day 23 focuses on real-world monitoring and observability, implemented using Terraform for a serverless AWS application.

This project demonstrates how production systems are observed, monitored, and alerted on — not just deployed.

🎯 Objective

Build a complete observability stack for a serverless application using Terraform, covering:

Logs

Default & custom metrics

Dashboards

Alarms

Notifications

Failure simulation

All infrastructure and monitoring components are managed as Infrastructure as Code.

🧠 Application Overview

The monitored application is a serverless image processing pipeline:

User uploads an image to an S3 bucket

AWS Lambda is triggered automatically

Image is processed into multiple formats

Processed images are stored in a destination S3 bucket

Logs, metrics, and alerts track every step

🧱 AWS Services Used

AWS Lambda – Serverless compute

Amazon S3 – Source & destination buckets

Amazon CloudWatch

Logs

Metrics

Dashboards

Alarms

Amazon SNS – Alert notifications

AWS CloudTrail – API activity auditing

Terraform (Custom Modules) – Full automation

🧩 Terraform Project Structure
day23/
├── modules/
│   ├── s3/
│   ├── lambda/
│   ├── sns/
│   ├── cloudwatch_logs/
│   ├── cloudwatch_metrics/
│   ├── cloudwatch_alarms/
│   └── iam/
├── main.tf
├── variables.tf
├── outputs.tf
├── backend.tf
├── terraform.tfvars
├── build_layer.sh
└── README.md


Each module is responsible for a single concern, following real production standards.

📊 Monitoring & Observability Implementation
🔹 Logging (CloudWatch Logs)

Lambda execution logs captured automatically

Log metric filters created using regex patterns

Logs converted into custom CloudWatch metrics

Tracked events:

Application errors

Invalid file uploads

Processing failures

Access denied events

Successful executions

🔹 Custom Metrics

Custom metrics go beyond default Lambda metrics:

Images processed successfully

Failed processing attempts

Invalid file formats

Large file uploads

Execution duration breaches

These metrics provide application-level visibility.

🔹 Dashboards

A CloudWatch dashboard is created using Terraform (JSON definition).

Widgets include:

Lambda invocation count

Error rate

Execution duration

P99 latency

Concurrent executions

Custom error metrics

Log-derived trends

This dashboard reflects production monitoring needs.

🔹 Alarms & Alerts

Multiple alarm categories are defined:

❌ Lambda errors

⏱️ High execution time

🔥 Concurrency threshold breaches

🚫 Invalid file uploads

📉 Log-based failures

Alarm thresholds are fully configurable via variables.

🔹 Notifications (SNS)

SNS topics created per alert category

Email subscriptions configured

Alerts delivered in real time on alarm trigger

This completes the incident notification pipeline.

🐋 Docker-Based Lambda Layer Build

To ensure runtime compatibility:

Lambda dependencies (Pillow) are built using Docker

Matches AWS Lambda Linux runtime

Avoids local OS compatibility issues

Terraform deploys the generated layer automatically

🧪 Testing & Validation

The setup is validated by:

Uploading valid images → metrics increase

Uploading invalid files → error alarms trigger

Uploading large files → size alarms trigger

Uploading multiple files → concurrency alarms trigger

Email alerts received via SNS

Monitoring is proven, not theoretical.

🔑 Key Learnings

Monitoring is mandatory for production systems

Logs → Metrics → Alerts is the real pipeline

Default metrics are insufficient alone

P99 latency matters more than averages

Terraform can manage full observability stacks

Modular Terraform enables clean scalability

Alert tuning is as important as alert creation

🏁 Conclusion

Day 23 demonstrates how production AWS systems are operated.

Not just:

“Did it deploy?”

But:

“Is it healthy?”
“Is it fast?”
“Is it failing?”
“Will I know immediately?”

By implementing logs, metrics, dashboards, alarms, and notifications entirely using Terraform, this project reflects real DevOps and SRE practices.

⚠️ Cleanup Reminder

This project creates multiple AWS resources.

After testing:

terraform destroy


Also ensure S3 buckets are emptied before destroy to avoid errors.
