📘 Day 29 – Deploying EKS with Terraform & Implementing GitOps using ArgoCD
📌 Overview

Day 29 focuses on building a production-style Kubernetes environment on AWS using Terraform public modules and implementing GitOps principles with ArgoCD.

In this project, you will:

Provision an EKS cluster using Terraform public modules

Deploy a three-tier application (frontend, backend, database)

Configure persistent storage using Amazon EBS

Install and configure ArgoCD

Implement GitOps with automatic drift detection and reconciliation

This session combines Infrastructure as Code (IaC), Kubernetes, and GitOps into one complete workflow.

🏗️ Architecture Summary
Infrastructure Layer

VPC and networking components

EKS cluster

Worker nodes

EBS volumes for persistent storage

Application Layer

Frontend deployment

Backend deployment

Database deployment

Kubernetes services

Dedicated namespace

GitOps Layer

GitHub repository as the source of truth

ArgoCD continuously monitors and syncs cluster state

Automatic drift detection and reconciliation

🛠️ Tools & Technologies

Terraform

Amazon Elastic Kubernetes Service (EKS)

ArgoCD

Amazon Elastic Block Store (EBS)

Kubernetes

GitHub

📂 Project Structure
day-29/
│
├── main.tf
├── providers.tf
├── variables.tf
├── outputs.tf
│
├── kubernetes/
│   ├── namespace.yaml
│   ├── frontend.yaml
│   ├── backend.yaml
│   ├── database.yaml
│
└── argocd/
    └── application.yaml

🚀 Deployment Steps
1️⃣ Initialize Terraform
terraform init


This downloads required providers and modules.

2️⃣ Review Infrastructure Plan
terraform plan


Validates configuration and previews resources before deployment.

3️⃣ Apply Infrastructure
terraform apply


This provisions:

VPC and networking

EKS cluster

Worker nodes

EBS-backed storage

4️⃣ Configure kubectl

Update kubeconfig to access the cluster:

aws eks update-kubeconfig --region <region> --name <cluster-name>

5️⃣ Install ArgoCD
kubectl create namespace argocd
kubectl apply -n argocd -f <argocd-installation-manifest>


Expose ArgoCD using a LoadBalancer service to access the UI.

6️⃣ Configure GitOps Workflow

Connect ArgoCD to the GitHub repository

Create an Application resource

Enable auto-sync

Monitor deployment status

ArgoCD continuously ensures the cluster state matches the Git repository.

🔁 Drift Detection Demonstration

If someone manually changes a deployment:

kubectl scale deployment frontend --replicas=5


ArgoCD will:

Detect configuration drift

Automatically revert the replica count to the value defined in Git

This ensures:

Infrastructure consistency

Controlled deployments

Self-healing systems

💾 Persistent Storage

The database uses:

PersistentVolume (PV)

PersistentVolumeClaim (PVC)

Backed by Amazon EBS

This ensures:

Data persistence across pod restarts

Support for stateful applications

Production-grade reliability

⚠️ Best Practices

Pin Terraform module versions

Review public module source code

Avoid storing secrets in Git repositories

Integrate AWS Secrets Manager for secure secret handling

Implement RBAC for ArgoCD

Enable cluster autoscaling for scalability

🎯 Learning Outcomes

By completing Day 29, you will:

Deploy EKS using Terraform public modules

Manage Kubernetes workloads with Terraform

Implement GitOps using ArgoCD

Handle persistent storage in Kubernetes

Understand drift detection and automatic reconciliation
