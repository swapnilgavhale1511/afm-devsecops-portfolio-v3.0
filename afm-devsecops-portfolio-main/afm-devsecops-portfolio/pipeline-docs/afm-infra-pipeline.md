# 🏗️ AFM Infrastructure CI/CD Pipeline
### Terraform-driven AWS EKS Provisioning via GitLab CI/CD

---

## 🔰 Overview

The **AFM Infrastructure Pipeline** is responsible for provisioning and managing
the **entire AWS infrastructure stack** required to run the AFM platform.

Unlike manual or local Terraform workflows, **all infrastructure changes are executed exclusively through GitLab CI/CD**, enforcing:

- Reproducibility
- Auditability
- Controlled changes
- Enterprise-style workflows

This pipeline provisions:
- AWS networking prerequisites
- IAM roles and permissions
- Amazon EKS cluster
- Managed node group
- Kubernetes access configuration
- AWS Load Balancer Controller (post-provision)

---

## 🎯 Design Principles

The infrastructure pipeline was designed with the following principles:

- **No local Terraform execution**
- **CI/CD as the single source of truth**
- **Manual approval for destructive actions**
- **Cost-aware defaults**
- **Clear separation from application pipelines**

This mirrors how **platform and SRE teams** operate in real organizations.

---

## 🧱 Infrastructure Components Provisioned

### AWS Services
- Amazon EKS
- EC2 (managed node group)
- IAM (EKS, node, and controller roles)
- S3 (Terraform remote state)
- DynamoDB (Terraform state locking)
- Application Load Balancer (via controller)

---

## 🗂️ Terraform Structure

Terraform follows a **modular design**:

- `vpc` – Networking (default VPC usage for simplicity)
- `iam` – EKS and controller IAM roles
- `eks` – Cluster and node group
- `s3_dynamodb` – Remote backend (state & locking)

Key practices:
- Remote backend enforced
- State locking enabled
- Environment-aware variables
- No hardcoded credentials

---

## 🚦 GitLab CI/CD Pipeline Stages

### 1️⃣ Validate
- Terraform syntax validation
- Provider configuration checks
- Prevents invalid plans from progressing

---

### 2️⃣ Plan
- Generates Terraform execution plan
- Shows all resource changes
- Plan output reviewed before apply

---

### 3️⃣ Apply (Manual Approval)
- Requires human approval
- Prevents accidental infrastructure changes
- Applies the reviewed plan only

---

### 4️⃣ Post-Provision: ALB Controller Installation
- Installs **AWS Load Balancer Controller**
- Enables Kubernetes Ingress support
- Required for exposing AFM services via ALB

This step is intentionally separated to:
- Ensure EKS readiness
- Avoid race conditions
- Reflect real platform bootstrapping

---

### 5️⃣ Destroy (Manual & Isolated)
- Fully manual stage
- Used only for cost cleanup or rebuilds
- Never triggered automatically

---

## 🔐 Security & Access Control

- IAM roles created using least privilege
- Terraform access scoped via CI variables
- No AWS credentials stored in code
- IRSA enabled for Kubernetes controllers

---

## 💰 Cost-Aware Decisions

- Single-node EKS cluster (`t3.medium`)
- Default VPC to reduce complexity
- No NAT Gateway
- Manual destroy to prevent orphaned resources

These constraints were **intentional** and documented.

---

## 🧠 Why This Pipeline Matters

This pipeline demonstrates:
- Real Infrastructure-as-Code discipline
- Safe Terraform workflows
- Separation of infra and app concerns
- Production-aligned approval models
- Platform engineering mindset

It is **not a demo pipeline**, but a **controlled infrastructure system**.

---

## 🔍 How This Integrates with AFM Application Pipelines

- Infra pipeline runs **first**
- Outputs cluster and access configuration
- Application pipelines deploy AFM services into the provisioned EKS cluster
- No infra changes occur during app deployments

This clean separation avoids:
- Accidental infra drift
- Security misconfigurations
- Coupled failures

---

## 🔚 Final Takeaway

> The AFM infrastructure pipeline treats AWS infrastructure as a **managed product**, not a one-time setup.

This approach reflects how **modern DevOps and platform teams** operate at scale.
