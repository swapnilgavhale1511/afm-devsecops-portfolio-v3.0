# 🏗️ AFM Infrastructure CI/CD Pipeline

### Terraform-Driven AWS Infrastructure Provisioning via GitLab CI/CD

The **AFM Infrastructure Pipeline** provisions, configures, validates, and destroys the AWS infrastructure and Kubernetes platform foundation required to run the AFM GitOps DevSecOps platform.

Infrastructure is managed as **Infrastructure as Code (IaC)** using modular Terraform and executed through **GitLab CI/CD** rather than relying on local Terraform execution.

The pipeline combines:

**Terraform + GitLab CI/CD + Trivy IaC Security + AWS + Amazon EKS + Kubernetes Platform Add-ons**

The result is a reproducible, auditable, security-validated infrastructure lifecycle that can be provisioned, operated, and destroyed in a controlled manner.

---

# 📌 Pipeline Scope

The infrastructure pipeline is responsible for the platform foundation required before application GitOps delivery.

It covers:

- AWS networking
- Routing and internet connectivity
- Security groups and network controls
- IAM roles and policies
- Amazon ECR repositories
- Route 53 and ACM resources
- ALB access-log storage
- AWS Secrets Manager foundations
- Amazon RDS PostgreSQL
- Amazon EKS
- Dedicated EKS worker nodes
- EKS identity configuration
- EKS platform add-ons
- AWS Load Balancer Controller
- ExternalDNS
- Metrics Server
- Argo CD platform bootstrap
- Other Kubernetes platform components required by the AFM environment

The application CI/CD pipelines remain separate from the infrastructure pipeline.

---

# 🎯 Infrastructure Engineering Principles

The AFM infrastructure lifecycle follows these principles:

- **Infrastructure as Code** — AWS resources are defined in Terraform.
- **Git-driven infrastructure changes** — Terraform configuration is stored in Git.
- **CI/CD-driven execution** — infrastructure operations are performed through GitLab CI/CD.
- **Pre-provisioning security validation** — Terraform configuration is scanned using Trivy IaC.
- **Plan before apply** — infrastructure changes are reviewed through Terraform Plan.
- **Human-controlled Apply** — infrastructure changes require controlled execution.
- **Remote state** — Terraform state is stored centrally in Amazon S3.
- **Native S3 state locking** — Terraform uses S3 native locking through `use_lockfile = true`.
- **Least-privilege AWS access** — the CI runner authenticates to AWS through its IAM role rather than storing long-lived AWS access keys in the repository.
- **Modular infrastructure** — infrastructure is divided into reusable Terraform modules/layers.
- **Dependency-aware provisioning** — infrastructure is created in dependency order.
- **Reverse-order destruction** — resources are destroyed in an order that respects dependencies.
- **Cost-aware operation** — temporary development infrastructure can be destroyed when not required.
- **Environment-ready design** — the Terraform structure supports expansion toward multiple environments.

---

# 🏛️ High-Level Infrastructure Lifecycle

```text
Developer
    │
    ▼
GitLab Infrastructure Repository
    │
    ▼
GitLab CI/CD
    │
    ├── Pre-flight Checks
    ├── Terraform Init
    ├── Terraform Validate
    ├── Terraform Format Check
    ├── Trivy IaC Scan
    ├── Terraform Plan
    ├── Manual Approval
    ├── Terraform Apply
    └── Post-Apply Outputs
            │
            ▼
     AWS Infrastructure
            │
            ▼
      Amazon EKS Platform
            │
            ▼
Platform Add-ons / Argo CD Bootstrap
            │
            ▼
    GitOps-Ready EKS Platform

☁️ AWS Infrastructure Provisioned
Networking

The AFM environment uses a custom VPC:

VPC CIDR: 10.0.0.0/16
Public Subnets
Public Subnet A
10.0.1.0/24


Public Subnet B
10.0.2.0/24

Used for internet-facing infrastructure such as the Application Load Balancer.

Private Subnets
Private Subnet A
10.0.3.0/24


Private Subnet B
10.0.4.0/24

Used for private workloads such as:

Amazon EKS worker nodes
Amazon RDS
internal platform components
Network Components

Terraform provisions/configures:

VPC
Internet Gateway
Public Route Table
Private Route Table
NAT Instance
Security Groups
Network ACLs
subnet relationships

The development environment uses a NAT Instance (t3.micro) rather than a NAT Gateway as a cost-conscious architecture choice.

☸️ Amazon EKS Platform

The infrastructure pipeline provisions the Amazon EKS cluster used by the AFM platform.

The current architecture contains:

                 Amazon EKS Cluster
                         │
             ┌───────────┴───────────┐
             │                       │
             ▼                       ▼
      AFM Worker Node          APA Worker Node
         t3.medium                t3.medium
             │                       │
       namespace:               namespace:
        afm-bank             afm-platform-assistant
EKS Control Plane

The Kubernetes control plane is managed by AWS.

Terraform provisions the EKS cluster configuration and associated AWS resources required for the platform.

Dedicated Worker Nodes

Two dedicated worker nodes are used.

AFM Worker Node
Instance class: t3.medium
Purpose: AFM application workloads
Namespace: afm-bank

Hosts workloads such as:

frontend-ui
login-service
registration-service
auth-service
other AFM application pods
APA Worker Node
Instance class: t3.medium
Purpose: AFM Platform Assistant
Namespace: afm-platform-assistant

Hosts the APA workload independently from AFM application workloads.

This separation provides a clear workload boundary within the same EKS cluster.

🔐 EKS Identity Architecture

The infrastructure layer establishes the IAM and Kubernetes identity foundation required by platform workloads.

AFM Workload Identity — IRSA

AFM workloads use the established IRSA model where AWS workload access is required:

Kubernetes ServiceAccount
        │
        ▼
       IRSA
        │
        ▼
   OIDC Provider
        │
        ▼
      IAM Role
        │
        ▼
   AWS Resources
APA Workload Identity — EKS Pod Identity

APA uses a separate identity model:

APA Pod
   │
   ▼
Kubernetes ServiceAccount
   │
   ▼
EKS Pod Identity
   │
   ▼
APA Read-Only IAM Role
   │
   ▼
AWS APIs / Resources

The APA role is restricted to read-only operational access.

APA is not part of the infrastructure mutation path.


🧩 Terraform Architecture

Terraform is organized using a modular, layered structure.

The following is a representative Terraform organization used to illustrate the project structure; exact module and environment paths should follow the current infrastructure repository:

afm-infra-provisioning/
│
├── envs/
│   └── <environment>/
│
├── modules/
│   ├── vpc/
│   ├── iam/
│   ├── eks/
│   ├── rds/
│   ├── route53/
│   ├── acm/
│   ├── alb/
│   ├── secrets/
│   └── observability/
│
├── scripts/
│
└── .gitlab-ci.yml

The environment layer composes reusable modules and controls environment-specific configuration.

🧱 Layered Terraform Provisioning

The infrastructure is provisioned in dependency-aware layers rather than treating the entire AWS environment as one undifferentiated Terraform operation.

The major layers include:

Layer	Purpose
bootstrap	Terraform remote state backend and initial dependencies
network	VPC, subnets, route tables, Internet Gateway and NAT
global	Route 53 and ACM/global networking prerequisites
ecr	Amazon ECR repositories
alb_logs	ALB access-log S3 bucket
secrets	AWS Secrets Manager resources/foundations
database	Amazon RDS PostgreSQL
cluster_base	Amazon EKS cluster, worker nodes and identity/platform prerequisites
addons	Kubernetes/Helm platform components

The exact layer composition can evolve with the environment, but the dependency-oriented model remains the governing principle.

💾 Terraform Remote State

Terraform uses Amazon S3 for remote state.

Terraform
    │
    ▼
Amazon S3 Remote State
    │
    ├── Versioning
    ├── Encryption
    └── Native S3 Locking

The current implementation uses:

use_lockfile = true

Terraform therefore uses S3 native locking rather than the legacy DynamoDB locking pattern.

Remote State Characteristics

Remote state provides:

Shared state
Centralized state management
State persistence across CI jobs
Concurrent-operation protection through locking
Recoverability through versioned state
Separation between code and state

No local state file is treated as the authoritative infrastructure state.

🚦 GitLab CI/CD Infrastructure Pipeline

The Terraform lifecycle is executed by GitLab CI/CD.

The pipeline runs on the project's shared/self-managed GitLab runner infrastructure.

Pipeline Stages
1. Pre-flight Validation

The pipeline validates the execution environment before running Terraform.

Typical checks include:

AWS CLI availability
AWS authentication
Terraform version
Working directory validation
Required variables/configuration

Purpose:

Fail early when the CI environment is not ready.

2. Terraform Init

Terraform initializes the working directory and remote backend.

Responsibilities include:

Backend initialization
Provider installation
Module initialization
Dependency preparation

Example:

terraform init

The remote S3 backend is initialized before validation, scanning, planning, or applying infrastructure.

3. Terraform Validate

Terraform configuration is validated before infrastructure changes are planned.

Checks include:

Terraform syntax
Module configuration
Provider configuration
Variables
Outputs
Resource references

Example:

terraform validate

Purpose:

Catch configuration errors early before security scanning and planning.

4. Terraform Format Check

Terraform formatting is validated to maintain consistent HCL structure.

Example:

terraform fmt -check

Purpose:

Consistent configuration style
Cleaner code review
Reduced formatting noise
Standardized Terraform repositories
5. Trivy IaC Security Scan

Terraform configuration is scanned using Trivy IaC before infrastructure provisioning.

The scan evaluates Terraform configuration for security and configuration problems including:

AWS misconfigurations
Exposed or overly permissive resources
Security group issues
IAM policy risks
Encryption configuration issues
Network security weaknesses
Public exposure risks
Infrastructure best-practice violations

The scan generates report artifacts such as:

HTML
JSON

A configurable quality gate can stop the pipeline when findings exceed the accepted severity threshold.

Security Position
Terraform Code
      │
      ▼
Trivy IaC Scan
      │
 ┌────┴────┐
 │         │
PASS      FAIL
 │         │
 ▼         ▼
Plan     Pipeline Stops

This places infrastructure security validation before Terraform changes reach AWS.

6. Terraform Plan

Terraform generates an execution plan showing the intended infrastructure changes.

terraform plan

The plan allows the team to inspect:

Resources to create
Resources to modify
Resources to destroy
Configuration changes
Dependency resolution
Infrastructure state differences

The generated plan can be preserved as a CI artifact.

Important Principle

Plan is read-only.

No AWS infrastructure is changed by the planning stage.

7. Manual Approval

Infrastructure changes are intentionally controlled before Apply.

The workflow is:

Terraform Plan
      │
      ▼
Review / Approval
      │
 ┌────┴────┐
 │         │
Approve   Reject
 │         │
 ▼         ▼
Apply     Stop

This provides a human control point before modifying AWS infrastructure.

8. Terraform Apply

After approval, Terraform applies the reviewed configuration.

terraform apply

Terraform resolves resource dependencies and provisions/updates AWS infrastructure.

Typical resources include:

VPC
Subnets
Route Tables
NAT Instance
Security Groups
IAM Roles
ECR
RDS
EKS
EKS worker nodes
Route 53
ACM
Secrets Manager
ALB logging infrastructure
platform prerequisites

Terraform updates the remote state in Amazon S3 after successful execution.

9. Cluster and Platform Add-ons

Once the EKS control plane and worker infrastructure are ready, the platform bootstrap phase installs/configures the Kubernetes components required by the AFM environment.

These can include:

AWS Load Balancer Controller
ExternalDNS
Metrics Server
Argo CD
Kubernetes platform components
Observability components
External Secrets Operator
other environment-specific Helm releases

The Application Load Balancer is created and managed by the AWS Load Balancer Controller from Kubernetes Ingress resources; Terraform provisions the supporting platform prerequisites rather than treating the runtime ALB as a static Terraform-managed application resource.

The goal is to produce a:

GitOps-ready Amazon EKS platform

rather than immediately mixing application deployment into infrastructure provisioning.

🔄 Infrastructure → GitOps Handoff

The infrastructure pipeline prepares the platform.

The application/GitOps pipeline then consumes the resulting platform.

Infrastructure Pipeline
        │
        ▼
AWS + EKS Platform Ready
        │
        ▼
Cluster Endpoint / IDs / IAM Outputs
        │
        ▼
GitOps / Application Pipelines
        │
        ▼
Argo CD
        │
        ▼
Amazon EKS Workloads

This provides a clean separation.

Infrastructure Pipeline

Responsible for:

AWS
networking
EKS
IAM
RDS
secrets foundations
platform add-ons
Argo CD/platform bootstrap
Application / GitOps Pipeline

Responsible for:

application source
container image
security scans
ECR image publishing
Kubernetes manifests
GitOps desired state
Argo CD synchronization
🧩 Infrastructure Pipeline vs Application Pipeline
Area	Infrastructure Pipeline	Application Pipeline
Terraform	✅	❌
AWS VPC	✅	❌
EKS Cluster	✅	❌
Worker Nodes	✅	❌
RDS	✅	❌
IAM	✅	❌
ECR Repository	✅	Application image pushed
Kubernetes Add-ons	✅	❌
Docker Build	❌	✅
SonarQube	❌	✅
Trivy Container Scan	❌	✅
OWASP ZAP	❌	✅
GitOps Manifest Update	❌	✅
Argo CD Application Deployment	Platform bootstrap	✅

This separation avoids coupling AWS infrastructure changes to ordinary application releases.

💥 Destroy Workflow

Infrastructure destruction is intentionally isolated from normal Apply operations.

Supported actions include:

plan
apply
destroy
destroy-all

Destroy operations require deliberate execution.

The infrastructure is removed in dependency-aware reverse order.

A simplified model is:

Application / Add-ons
        ↓
Argo CD / Helm Components
        ↓
EKS Platform Components
        ↓
EKS Worker Nodes
        ↓
Amazon EKS
        ↓
RDS / ECR / Secrets / ALB Logs
        ↓
Network Dependencies
        ↓
NAT / Route Tables / Subnets / VPC

The exact destruction order is ultimately governed by Terraform's dependency graph.

Why Reverse Dependency Order Matters

Destroying foundational resources too early can break dependent resources or produce unnecessary failures.

Terraform's dependency graph determines the actual execution order.

💰 Cost-Aware Development Lifecycle

AFM is operated as a cost-aware development environment.

Some foundational resources may remain available while short-lived platform components are created and destroyed as required.

For development/testing, the platform can be rebuilt using the infrastructure pipeline rather than keeping the full EKS environment permanently running.

This allows:

Provision
   ↓
Validate
   ↓
Deploy / Test
   ↓
Observe
   ↓
Destroy temporary resources

The strategy reduces unnecessary AWS consumption while preserving reproducibility.

🔐 Security Controls in Infrastructure Provisioning

Security is integrated into the infrastructure lifecycle rather than treated as a separate manual activity.

Infrastructure Security
Terraform validation
Terraform formatting
Trivy IaC scanning
IAM least privilege
Private EKS worker placement
Security Groups
Network ACLs
Encryption where configured
S3 remote state protection
Identity
IAM roles
EKS OIDC / IRSA for applicable workloads
EKS Pod Identity for APA
No long-lived AWS credentials embedded in workloads
Secrets
AWS Secrets Manager
External Secrets Operator
Kubernetes Secrets
No application secrets stored in Git
📊 Platform Outputs

The infrastructure pipeline produces outputs consumed by subsequent platform/application workflows.

Typical outputs include:

EKS Cluster Endpoint
VPC ID
Subnet IDs
Security Group IDs
ECR Repository URLs
RDS Endpoint
IAM Role ARNs
OIDC Provider information
Terraform state information
Other environment-specific resource identifiers

These outputs are exposed through controlled Terraform outputs/artifacts where required.

🧪 Development / Environment Readiness

The Terraform implementation is structured to support environment expansion.

The project is not limited to a hardcoded single-environment model.

A representative environment pattern is:

environments/
├── dev/
├── test/
├── uat/
└── prod/

with reusable Terraform modules.

The current portfolio implementation focuses primarily on the development environment while maintaining an environment-ready architecture.

🛠️ Tooling
Infrastructure
Terraform
AWS Provider
AWS CLI
CI/CD
GitLab CI/CD
GitLab Runner
Infrastructure Security
Trivy IaC
Cloud Platform
Amazon VPC
Amazon EKS
Amazon EC2
Amazon RDS PostgreSQL
Amazon ECR
Amazon S3
Route 53
AWS Certificate Manager
AWS Secrets Manager
IAM
CloudWatch
Kubernetes Platform
Kubernetes
Helm
Argo CD
AWS Load Balancer Controller
ExternalDNS
Metrics Server
External Secrets Operator
🔁 Complete Infrastructure Lifecycle
Developer Commit
       │
       ▼
GitLab Infrastructure Repository
       │
       ▼
GitLab CI/CD
       │
       ├── Pre-flight Checks
       │
       ├── Terraform Init
       │
       ├── Terraform Validate
       │
       ├── Terraform Format Check
       │
       ├── Trivy IaC Scan
       │
       ├── Terraform Plan
       │
       ├── Manual Approval
       │
       ├── Terraform Apply
       │
       └── Post-Apply Outputs
              │
              ▼
       AWS Infrastructure
              │
              ▼
        Amazon EKS Ready
              │
              ▼
  Platform Add-ons / Argo CD Bootstrap
              │
              ▼
       GitOps-Ready Platform
              │
              ▼
      Application Pipelines
              │
              ▼
            Argo CD
              │
              ▼
       AFM / APA Workloads
🚦 Supported Pipeline Actions

The infrastructure pipeline supports controlled lifecycle actions such as:

Action	Purpose
plan	Preview infrastructure changes
apply	Provision/update infrastructure
destroy	Destroy selected infrastructure
destroy-all	Full development environment teardown

Every destructive action is deliberate and separately controlled.

📌 Key Engineering Characteristics
Reproducible

Infrastructure can be recreated from version-controlled Terraform configuration.

Auditable

Changes originate from Git and execute through GitLab CI/CD.

Security-Checked

Terraform configuration is scanned with Trivy IaC before provisioning.

Modular

Reusable Terraform modules separate networking, IAM, EKS, database, secrets, Route 53, ALB logs and platform components.

Dependency-Aware

Provisioning and destruction follow infrastructure dependencies.

Cost-Aware

Development infrastructure can be destroyed and recreated when required.

GitOps-Ready

The infrastructure pipeline prepares the EKS platform for subsequent Argo CD-based workload deployment.

APA Platform Support

The infrastructure provisions the Amazon EKS platform and workload identity required to host the read-only APA operations workload.

APA remains outside the infrastructure mutation path.

🔗 Relationship with AFM Application and GitOps Pipelines

The AFM platform is intentionally separated into infrastructure, application CI/CD, and GitOps responsibilities.

                    ┌─────────────────────────────┐
                    │ Infrastructure Pipeline     │
                    │ Terraform + Trivy IaC       │
                    └─────────────┬───────────────┘
                                  │
                                  ▼
                         AWS / Amazon EKS
                                  │
                    ┌─────────────┴─────────────┐
                    │                           │
                    ▼                           ▼
            Application CI/CD             Platform / GitOps
          GitLab → Build → Scan         GitOps Repo → Argo CD
          → ECR → GitOps Update               │
                    │                         │
                    └────────────┬────────────┘
                                 ▼
                            Amazon EKS
                                 │
                 ┌───────────────┴───────────────┐
                 │                               │
                 ▼                               ▼
          AFM Application                    APA
          `afm-bank`                   `afm-platform-assistant`

Infrastructure prepares the platform.

GitLab application pipelines build and secure application artifacts.

GitOps defines desired Kubernetes state.

Argo CD reconciles that desired state with EKS.

APA provides read-only operational intelligence over approved platform evidence.


🧠 What This Demonstrates

The AFM Infrastructure Pipeline demonstrates more than basic Terraform execution.

It demonstrates an end-to-end infrastructure lifecycle involving:

Version-controlled infrastructure
CI/CD-driven Terraform
Infrastructure security scanning
Remote state management
Native S3 state locking
Modular AWS infrastructure
EKS platform provisioning
IAM and workload identity
Kubernetes platform bootstrapping
Environment-ready architecture
Controlled infrastructure changes
Dependency-aware destruction
Cost-aware development operations
Clear separation between infrastructure and application delivery
GitOps-ready Kubernetes platform
🔚 Final Takeaway

The AFM Infrastructure Pipeline treats AWS infrastructure as version-controlled, security-validated, CI/CD-managed infrastructure rather than a manually configured environment. Terraform defines the desired infrastructure, GitLab CI/CD provides the controlled execution mechanism, Trivy validates infrastructure security before provisioning, Amazon S3 provides remote state and native locking, and the resulting Amazon EKS platform becomes the foundation for GitOps-based application delivery and the read-only APA operations workload.