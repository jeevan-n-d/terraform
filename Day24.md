🚀 Terraform Day 24: Highly Available Web Application on AWS (ALB + ASG)

Day 24 focuses on building a production-ready, highly available web application architecture on AWS using Terraform.

This project demonstrates how real applications are hosted in the cloud, following AWS best practices for availability, scalability, security, and automation.

🎯 Project Objective

Provision a fault-tolerant web application that:

Runs across multiple Availability Zones

Uses Application Load Balancer (ALB) for traffic distribution

Scales automatically using Auto Scaling Groups (ASG)

Keeps application servers in private subnets

Uses NAT Gateway for secure outbound access

Deploys a Dockerized Django application on EC2

Is fully managed using Terraform

🧱 Architecture Overview
🌐 Networking

Custom VPC

Public subnets

Application Load Balancer

NAT Gateway

Private subnets

EC2 instances

Internet Gateway

Route tables for public & private routing

⚖️ Load Balancing

Application Load Balancer

Target Groups with health checks

Listener forwarding HTTP traffic to EC2

🔄 Compute & Scaling

Launch Template

Auto Scaling Group

Minimum, maximum, desired capacity

CPU-based scaling policies

Multi-AZ EC2 distribution

🐳 Application Deployment

Django application

Docker installed via EC2 user data

Container launched automatically on instance boot

🔐 Security Design

EC2 instances do not have public IPs

Application traffic flows only through ALB

Private subnets protect application servers

NAT Gateway enables outbound internet access only

SSH access handled securely using EC2 Instance Connect Endpoint

📂 Terraform Project Structure
day24/
├── provider.tf
├── variables.tf
├── outputs.tf
├── vpc.tf
├── security_groups.tf
├── alb.tf
├── launch_template.tf
├── autoscaling.tf
├── userdata.sh
└── README.md


Terraform automatically detects all .tf files and builds the complete infrastructure.

⚙️ Terraform Workflow
terraform init
terraform plan
terraform apply


Terraform provisions:

VPC and subnets

Load balancer & target groups

Auto Scaling Group

EC2 instances with Dockerized app

Routing & networking components

🧪 Testing & Validation
✅ High Availability

Application remains accessible even if one EC2 instance fails

ALB routes traffic only to healthy instances

Multi-AZ setup ensures fault tolerance

🔄 Autoscaling

Load tested using Apache JMeter

ASG reacts to CPU utilization

Observed that CPU alone may not reflect real traffic load

Highlights need for memory/request-based metrics in production

🔐 Private Subnet Access

Direct SSH blocked

EC2 Instance Connect Endpoint used for secure access

No public exposure of EC2 instances

🧠 Key Learnings

High availability requires multi-AZ design

Load balancers are mandatory for production systems

Private subnets significantly improve security

NAT Gateways enable safe outbound access

CPU-only autoscaling is often insufficient

Docker simplifies application consistency

Terraform cleanly manages complex infrastructure

⚠️ Cost Awareness

This project creates:

Application Load Balancer

NAT Gateway

Multiple EC2 instances

After testing, always clean up:

terraform destroy


Also ensure resources are fully terminated to avoid unexpected charges.

🏁 Conclusion

Day 24 demonstrates how production web applications are hosted on AWS.

This is not a demo setup — it reflects:

Real traffic handling

Failure tolerance

Secure networking

Automated scaling

By combining ALB, Auto Scaling Groups, private networking, Docker, and Terraform, this project showcases enterprise-grade infrastructure as code.
