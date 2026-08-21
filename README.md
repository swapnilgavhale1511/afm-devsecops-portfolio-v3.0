# 🚀 AFM v3 — GitOps DevSecOps Cloud Project
### **Portfolio by Swapnil Gavhale**
-----------------------------------------------------------------------

> **AFM = Application / Feature / Microservice**

> AFM v3 is a production-inspired cloud-native **DevOps, DevSecOps, GitOps and platform engineering project** built around a small reference application, **AFM Bank**, and evolved into an AWS EKS-based platform with CI/CD, Infrastructure as Code, security, observability, SRE practices and a read-only AI-assisted operations layer.

![Java](https://img.shields.io/badge/Java-17-orange?logo=openjdk)
![Spring Boot](https://img.shields.io/badge/Spring_Boot-3.3.11-6DB33F?logo=springboot)
![Terraform](https://img.shields.io/badge/Terraform-1.x-844FBA?logo=terraform)
![AWS](https://img.shields.io/badge/AWS-Cloud-FF9900?logo=amazonaws)
![Kubernetes](https://img.shields.io/badge/Kubernetes-EKS-326CE5?logo=kubernetes)
![GitLab CI](https://img.shields.io/badge/GitLab-CI%2FCD-FC6D26?logo=gitlab)
![Argo CD](https://img.shields.io/badge/Argo_CD-GitOps-EF7B4D?logo=argo)
![Prometheus](https://img.shields.io/badge/Prometheus-Monitoring-E6522C?logo=prometheus)
![Grafana](https://img.shields.io/badge/Grafana-Dashboards-F46800?logo=grafana)
![OWASP ZAP](https://img.shields.io/badge/OWASP-ZAP-00549E)
![Trivy](https://img.shields.io/badge/Trivy-Security-1904DA)

---

## 🚀 Project at a Glance

AFM v3 demonstrates the complete lifecycle of a cloud-native application and the platform built around it.

```text
Application
    ↓
Source Control
    ↓
CI/CD + Security
    ↓
Container Image
    ↓
Infrastructure as Code
    ↓
AWS / EKS
    ↓
GitOps / Argo CD
    ↓
Observability / SRE
    ↓
Read-only AI-assisted Operations
```

The project is primarily about **how the platform is engineered and operated**. The reference application supplies a realistic workload for the platform; it is not intended to represent a real banking system.
-----------------------------------------------------------------------

### What this repository covers

- Application and microservice architecture
- AWS cloud infrastructure
- Terraform Infrastructure as Code
- EKS and Kubernetes
- GitLab CI/CD
- Docker and Amazon ECR
- Argo CD GitOps
- DevSecOps security controls
- Secrets management and workload identity
- Prometheus, Grafana, Alertmanager and YACE
- SRE-oriented operational dashboards
- Troubleshooting and operational workflows
- APA — AFM Platform Assistant
- Static RAG and dynamic read-only AWS/Kubernetes access
- Engineering challenges, limitations and roadmap
- A separate **Engineering Decisions & Trade-offs** section at the end

---

## 🧱 What Was Built

AFM v3 is implemented as a collection of independently versioned application, infrastructure, GitOps, observability and AI/platform components.

| Capability | Implemented components |
|---|---|
| Cloud foundation | AWS VPC, public/private subnets, routing, security groups, Internet Gateway, NAT Instance |
| Kubernetes platform | Amazon EKS, worker nodes, EKS addons, workload scheduling and capacity configuration |
| Application runtime | Java 17, Spring Boot, four AFM application components, executable JARs and Docker images |
| Database | Amazon RDS PostgreSQL |
| Application ingress | Route 53, ACM, Application Load Balancer and AWS Load Balancer Controller |
| Infrastructure as Code | Terraform modules, environment configuration, remote S3 state and S3-native locking |
| Container registry | Amazon ECR |
| CI/CD | GitLab CI/CD pipelines for application and infrastructure workflows |
| GitOps | Git-based Kubernetes desired state with Argo CD reconciliation |
| Security | SonarQube, Trivy container scanning, Trivy IaC, OWASP ZAP, IAM, RBAC and workload identity |
| Secrets | AWS Secrets Manager, External Secrets and Kubernetes Secret integration |
| Observability | Prometheus, Grafana, Alertmanager, YACE, CloudWatch and Slack notifications |
| SRE operations | Service health, request/error/latency metrics, SLO-oriented views, alerting and incident runbooks |
| AI-assisted operations | APA, Streamlit, static RAG, ChromaDB, OpenAI API, Kubernetes API and Boto3 |
| Platform safety | Strictly read-only APA permissions for AWS and Kubernetes access |

The project therefore contains both the **platform infrastructure** and the **delivery/operations systems required to operate the reference workload on that platform**.


---

## 📌 What is AFM?

**AFM originally stood for Application / Feature / Microservice.**

The project started with application/microservice development and progressively evolved into a complete cloud-native delivery and operations platform.

A small reference application, **AFM Bank**, is used only as a demo workload. It is **not a banking product** and the project is not intended to model real banking business processes.

```text
                         AFM v3 PLATFORM
                               │
             ┌─────────────────┼─────────────────┐
             │                 │                 │
             ▼                 ▼                 ▼
       Reference App          APA          Observability
        (AFM Bank)      Platform Assistant    & SRE
             │                 │                 │
             └─────────────────┴─────────────────┘
                               │
                               ▼
                    AWS / EKS / GitOps Platform
```

Where:

- **AFM v3** = the overall cloud, DevOps, DevSecOps, GitOps and platform engineering project
- **AFM Bank** = the small reference/demo application
- **APA** = AFM Platform Assistant
- **Observability** = Prometheus, Grafana, Alertmanager, YACE and CloudWatch-based monitoring

The primary portfolio story is **the AFM v3 platform and its engineering lifecycle**, not the banking domain.

---

# 🎯 Problem Statement & Motivation

Modern cloud-native platforms require more than simply running containers or
creating CI/CD pipelines. As application services, infrastructure,
security requirements and operational dependencies increase, teams need
repeatable delivery, controlled deployments, infrastructure automation,
security validation and actionable observability.

AFM v3 was developed as an evolution of the original AFM platform to address
these engineering challenges through a GitOps-based, DevSecOps and
SRE-oriented architecture.

The project focuses on:

- Repeatable AWS infrastructure provisioning through Terraform
- Independent CI/CD pipelines for application services
- Git-based Kubernetes deployment management through Argo CD
- Security validation across application, container and Infrastructure-as-Code
  delivery stages
- Centralized application, Kubernetes, AWS, ALB and RDS observability
- Automated alerting and operational incident visibility
- Cost-aware management of an ephemeral EKS development platform
- Read-only AI-assisted platform investigation through APA

The goal is not to demonstrate individual tools in isolation, but to
demonstrate how these technologies work together as an operational platform.

### Motivation Behind AFM v3

AFM v3 was created to demonstrate the evolution from a traditional
CI/CD-oriented application platform into a more complete cloud-native
DevOps platform.

The project intentionally emphasizes:

- **Automation over manual operations**
- **GitOps over direct Kubernetes deployment**
- **Security throughout the delivery lifecycle**
- **Observability as an operational requirement**
- **Failure analysis and recovery**
- **Cost-aware infrastructure design**
- **Operational visibility through AI-assisted read-only investigation**

---

# 🧬 AFM v2 → v3 Evolution

AFM v3 represents the evolution of the original AFM application and DevSecOps
foundation into a broader cloud-native delivery, GitOps, observability and
platform engineering environment.

The project did not start with the final architecture. It evolved through
application development, CI/CD, containerization, AWS infrastructure,
Kubernetes, security, observability, GitOps and finally AI-assisted
read-only operations.

| Area | AFM v2 | AFM v3 |
|---|---|---|
| Application repositories | Earlier project structure | Independent repository for each AFM service |
| CI/CD | CI/CD-oriented delivery | CI builds, validates and publishes artifacts |
| Kubernetes deployment | CI/CD/manual deployment workflow | Argo CD GitOps reconciliation |
| Infrastructure | Terraform-based AWS infrastructure | Terraform with separated ephemeral/persistent lifecycle |
| Deployment model | Standard deployment | Controlled Blue-Green deployment for `afm-auth-service` |
| Security | Core application scanning | SonarQube + Trivy + Trivy IaC + OWASP ZAP |
| Observability | Prometheus / Grafana foundation | Prometheus + Grafana + Alertmanager + YACE + CloudWatch |
| AWS monitoring | Basic AWS monitoring | RDS and ALB metrics integrated into Prometheus/Grafana |
| Alerting | Basic monitoring | Alertmanager → Slack |
| Secrets | Earlier secret-management approach | AWS Secrets Manager + External Secrets |
| AWS workload identity | Earlier model | IRSA for `afm-auth-service` + EKS Pod Identity for APA |
| Kubernetes capacity | Earlier single-node constraint | Two-node EKS platform with dedicated APA workload capacity |
| DNS / TLS | Earlier constrained implementation | Route 53 + ACM + ALB |
| AI operations | Not present | APA with static RAG + dynamic read-only AWS/Kubernetes access |

### Evolution in one view


```text
AFM v2
  ↓
Application + CI/CD Foundation
  ↓
AFM v3
  ↓
Terraform + AWS + EKS
  ↓
DevSecOps
  ↓
Argo CD GitOps
  ↓
Prometheus + Grafana + Alerting
  ↓
SRE-oriented Operations
  ↓
Static RAG
  ↓
Dynamic APA
  ↓
Read-only AWS + Kubernetes Operations
```
---

## 🎯 Project Objectives

AFM v3 demonstrates an end-to-end engineering lifecycle across:

- Cloud infrastructure
- Infrastructure as Code
- Linux and containers
- Kubernetes / EKS
- Application delivery
- CI/CD
- GitOps
- DevSecOps
- Security and secrets management
- Observability
- SRE-oriented operations
- Platform engineering
- AI-assisted, read-only operations

The main body of this README explains **what the project contains, how the components work together, and how software and infrastructure move through the platform**. Detailed architectural reasoning is intentionally consolidated at the end.

---

# 🔄 How AFM v3 Works End-to-End

The complete lifecycle connects source code, infrastructure, security, deployment, observability and read-only operational assistance.

```text
                           DEVELOPER
                               │
                               ▼
                    GitLab Application Repo
                               │
                               ▼
                         GitLab CI/CD
                               │
             ┌─────────────────┼──────────────────┐
             │                 │                  │
             ▼                 ▼                  ▼
          Maven            SonarQube            Trivy
             │                 │                  │
             └─────────────────┴──────────────────┘
                               │
                               ▼
                         Docker Build
                               │
                               ▼
                          Amazon ECR
                               │
                               ▼
                       GitOps Repository
                               │
                               ▼
                            Argo CD
                               │
                               ▼
                           Amazon EKS
                    ┌──────────┼───────────┐
                    │          │           │
                    ▼          ▼           ▼
                AFM Apps     APA     Observability
                    │          │           │
                    ▼          │      Prometheus
                   RDS         │      Grafana
                               │      Alertmanager
                               │      YACE
                               │          │
                               │        Slack
                               │
                    ┌──────────┴──────────┐
                    ▼                     ▼
              Kubernetes API           AWS APIs
                    │                     │
                    └──────────┬──────────┘
                               ▼
                         Retrieved Evidence
                               │
                               ▼
                          OpenAI API
                               │
                               ▼
                         Grounded Answer
```

### Infrastructure lifecycle

```text
Terraform Source
      ↓
GitLab Infrastructure Pipeline
      ↓
Terraform Validation
      ↓
Trivy IaC
      ↓
Terraform Plan
      ↓
Controlled Apply
      ↓
AWS Resources
      ↓
EKS / RDS / Networking / Security
```

### Application delivery lifecycle

```text
Application Commit
      ↓
GitLab CI/CD
      ↓
Build + Unit Tests
      ↓
SonarQube
      ↓
Docker Build
      ↓
Trivy Container Scan
      ↓
Amazon ECR
      ↓
GitOps Manifest Update
      ↓
Argo CD
      ↓
Amazon EKS
      ↓
OWASP ZAP
      ↓
Quality Gate
```

### Operational lifecycle

```text
Deploy
  ↓
Observe
  ↓
Detect
  ↓
Alert
  ↓
Investigate
  ↓
Understand
  ↓
Engineer-controlled remediation
  ↓
Verify
```

APA participates in the **Understand** stage by retrieving project documentation and current read-only AWS/Kubernetes state. It does not perform the remediation step.


---

# 🧭 AFM v3 Project Evolution

AFM was not designed as a finished architecture on day one.

It evolved incrementally:

```text
Simple Application / Microservice
             │
             ▼
       GitLab CI/CD
             │
             ▼
        Containerization
             │
             ▼
      AWS Infrastructure
             │
             ▼
       Amazon EKS
             │
             ▼
      Amazon RDS PostgreSQL
             │
             ▼
    Route 53 + ACM + ALB
             │
             ▼
       DevSecOps Controls
             │
             ▼
        Argo CD GitOps
             │
             ▼
 Prometheus + Grafana + Alerts
             │
             ▼
        SRE Operations
             │
             ▼
       Static APA / RAG
             │
             ▼
     Dynamic APA on EKS
             │
             ▼
Read-Only AWS + Kubernetes Operations
```

This evolution is a central part of the project rather than an afterthought.

---

# 🏗️ Final Architecture

```text
                              Internet / Client
                                      │
                                      │ DNS lookup
                                      ▼
                                   Route 53
                                      │
                                      │ DNS → ALB
                                      ▼
                           Application Load Balancer
                                      │
                                      │ HTTPS :443
                                      │
                                      └── ACM TLS Certificate
                                          (TLS termination)
                                      │
                                      ▼
                               Kubernetes Ingress
                                      │
                                      ▼
                              ┌────────────────────┐
                              │  afm-frontend-ui   │
                              │ UI + Reverse Proxy │
                              └─────────┬──────────┘
                                        │
                              ┌─────────┴─────────┐
                              │                   │
                              ▼                   ▼
                      Registration             Login
                         Service              Service
                              │                   │
                              └─────────┬─────────┘
                                        ▼
                                afm-auth-service
                               Identity / JWT Core
                                        │
                              ┌─────────┴─────────┐
                              │                   │
                              ▼                   ▼
                         PostgreSQL         AWS Secrets
                            RDS               Manager
```

The platform around the application:

```text
                         AWS
                          │
          ┌───────────────┼────────────────┐
          │               │                │
        EKS              RDS              S3
          │                              State/
          │                               Logs
     ┌────┴─────┐
     │          │
     ▼          ▼
 AFM Apps      APA
              │
       ┌──────┴────────┐
       ▼               ▼
   Static RAG     Dynamic Access
       │            │       │
   ChromaDB       K8s      AWS
                    │       │
                    └───┬───┘
                        ▼
                   OpenAI API
```

---

# 🧩 Application Architecture

AFM Bank uses four application components.

| Component | Role | Responsibility |
|---|---|---|
| `afm-frontend-ui` | UI + Reverse Proxy | Serves the browser-facing UI and proxies frontend API requests to backend services |
| `afm-registration-service` | Registration Service | Handles user registration workflow |
| `afm-login-service` | Login Service | Handles user login workflow |
| `afm-auth-service` | Authentication / Identity Service | Handles authentication, JWT processing, persistence and AWS Secrets Manager access |

The resulting application flow is:

```text
Browser
   │
   ▼
afm-frontend-ui
(UI + Reverse Proxy)
   │
   ├──────────────► Registration Service
   │
   └──────────────► Login Service
                         │
                         ▼
                  Auth Service
                    │       │
                    ▼       ▼
                  RDS    Secrets Manager
```

---

# 🔄 WAR → Executable JAR Evolution

Earlier application packaging used a traditional WAR-style model.

AFM v3 moved toward:

```text
Spring Boot
    ↓
Executable JAR
    ↓
Docker Image
    ↓
Amazon EKS
```

This removes the need to manage a separate application-server lifecycle inside the deployment environment.

It is a small but meaningful modernization decision.

---

# ☁️ AWS Architecture

AFM uses AWS as the cloud platform.

Major services include:

- Amazon VPC
- Amazon EC2
- Amazon EKS
- Amazon RDS PostgreSQL
- Amazon ECR
- Amazon S3
- AWS Secrets Manager
- IAM
- EKS workload identity
- Route 53
- AWS Certificate Manager (ACM)
- Application Load Balancer
- CloudWatch

These services form the cloud foundation on which the application, GitOps, observability and APA layers run.
# 💰 Cost-Conscious Infrastructure Design

The platform is deliberately designed for development/testing and portfolio demonstration rather than production HA.

The project uses:

- Cost-conscious EC2 worker nodes
- NAT Instance rather than NAT Gateway
- Ephemeral EKS infrastructure
- Controlled infrastructure destruction
- Persistent storage only where required
- Long-lived AWS resources kept selectively where recreating them would be unnecessary or expensive

The development EKS environment can be destroyed when it is not required.

This results in a lower-cost environment with intentionally reduced availability and redundancy compared with a production multi-AZ architecture.
# 🌐 Networking

The platform uses a Terraform-managed VPC with:

- Public subnets
- Private subnets
- Route tables
- Internet Gateway
- NAT Instance
- Security Groups
- EKS networking
- Application Load Balancer integration

The design separates public ingress from private application/database resources and provides the network foundation for EKS workloads.
# 🌍 DNS, HTTPS and Public Traffic

The public application uses:

```text
afmcloud.in
```

Traffic flow:

```text
Client / Browser
      │
      │ DNS lookup
      ▼
   Route 53
      │
      │ DNS → ALB
      ▼
Application Load Balancer
      │
      │ HTTPS :443
      │
      └── ACM TLS Certificate
          (TLS termination)
      │
      ▼
AWS Load Balancer Controller
      │
      ▼
Kubernetes Ingress
      │
      ▼
Frontend Service
      │
      ▼
Application Pod
```

---

# 🏗️ Infrastructure as Code — Terraform

Terraform is used to provision AWS infrastructure reproducibly.

The project separates reusable modules from environment-specific configuration.

Major areas include:

- VPC and networking
- IAM
- Amazon EKS
- EKS addons
- Amazon ECR
- Amazon RDS PostgreSQL
- Route 53
- ACM
- ALB logging
- AWS Secrets Manager
- External Secrets
- EKS workload identity

The infrastructure is version-controlled and executed through CI-driven workflows rather than relying on manually created cloud resources.
# 🗄️ Terraform Remote State

Terraform state is stored in Amazon S3.

The current implementation uses S3-native locking:

```hcl
use_lockfile = true
```

DynamoDB is not used for Terraform state locking in the current implementation.

# ☸️ EKS Pod Capacity Engineering

One of the practical engineering challenges was Kubernetes pod capacity on cost-conscious worker nodes.

The platform evolved through:

```text
Cost-conscious worker node
          ↓
Pod/IP capacity pressure
          ↓
VPC CNI Prefix Delegation
          ↓
Custom max-pods configuration
          ↓
Launch Template configuration
          ↓
Additional node capacity
          ↓
APA workload separation
```

---

# 🖥️ Worker Node Separation

The final environment uses separate worker capacity for APA.

```text
Amazon EKS
    │
    ├── Platform/Application Node
    │       └── AFM workloads
    │
    └── APA Node
            └── APA workload
```

APA introduces a Python/LLM-oriented workload in addition to the Java application and observability components. The separate worker capacity provides workload isolation and reduces resource contention.
# 🔐 Secrets Management

Sensitive application values are not hard-coded into source code or container images.

The application secret flow is:

```text
AWS Secrets Manager
        ↓
External Secrets
        ↓
Kubernetes Secret
        ↓
Application
```

The authentication service uses AWS Secrets Manager for sensitive database/JWT-related secret material.

---

# 🔑 Workload Identity — IRSA vs EKS Pod Identity

The project uses workload-specific AWS identity mechanisms.

## AFM Authentication Service

```text
afm-auth-service
       ↓
     IRSA
       ↓
    IAM Role
       ↓
AWS Secrets Manager / AWS APIs
```

IRSA is used for the authentication service because it requires AWS workload access as part of its database/Secrets Manager responsibilities.

## APA

```text
APA Pod
   ↓
EKS Pod Identity
   ↓
Dedicated IAM Role
   ↓
Read-only AWS APIs
```

APA uses EKS Pod Identity for its AWS read-only integrations.

The two workloads therefore have distinct AWS identity paths aligned with their responsibilities, while avoiding long-lived static AWS access keys inside containers.
# 🔐 Kubernetes RBAC for APA

APA is intentionally not a Kubernetes administrator.

```text
APA Pod
   ↓
Kubernetes RBAC
   ↓
Read-only permissions
   ↓
Kubernetes API
```

Supported read operations include information such as:

- Nodes
- Pods
- Deployments
- Services
- Namespaces
- Events
- Workload status

The permission model is designed around the principle of least privilege.

---

# 🛡️ DevSecOps

Security is integrated into both application and infrastructure delivery.

## Application delivery

```text
Commit
  ↓
Pre-cleanup
  ↓
Maven Build / Test
  ↓
SonarQube
  ↓
Docker Build
  ↓
ECR
  ↓
Trivy Container Scan
  ↓
GitOps Update
  ↓
Argo CD Deployment
  ↓
OWASP ZAP
  ↓
Quality Gate
```

## Infrastructure delivery

```text
Terraform
   ↓
Validation
   ↓
Trivy IaC
   ↓
Terraform Plan
   ↓
Controlled Apply
```
Trivy IaC is also used as a security feedback mechanism for Terraform configuration. 
During development, the scan identified EKS control-plane exposure and disabled secrets 
encryption as findings requiring evaluation. These findings were reviewed against the 
ephemeral development environment and are documented in the Engineering Decisions & 
Trade-offs section.

---

# 🔄 GitOps with Argo CD

AFM v3 uses pull-based GitOps for Kubernetes desired state.

### Earlier approach

```text
Developer
   ↓
kubectl apply
   ↓
Kubernetes
```

### AFM v3 approach

```text
Developer
   ↓
GitLab CI/CD
   ↓
Container Image
   ↓
Amazon ECR
   ↓
GitOps Repository
   ↓
Argo CD
   ↓
Amazon EKS
```

Argo CD provides the deployment control plane for:

- Declarative Kubernetes deployment
- Git as the desired-state source
- Drift detection
- Reconciliation
- Deployment history
- Git-based rollback
- Separation between CI and CD

The core responsibility boundary is:

> **GitLab CI builds and publishes the artifact; Argo CD reconciles the Kubernetes desired state.**
# 🟢 Blue-Green Deployment — Authentication Service

`afm-auth-service` uses a controlled Blue-Green deployment model.

```text
                 Kubernetes Service
                        │
                        ▼
                   Active Color
                  ┌─────────────┐
                  │ Blue/Green  │
                  └─────────────┘
                     ▲       ▲
                     │       │
                  Active    Idle
                  Version   Version
```

Release flow:

1. Determine active color.
2. Deploy the new version to the idle color.
3. Validate the idle workload.
4. Switch the Kubernetes Service selector.
5. Restore the previous selector if rollback is required.

This is a controlled Blue-Green release pattern rather than an automated progressive-delivery system.
# 📊 Observability

The observability stack combines application, Kubernetes and AWS telemetry.

```text
Spring Boot
    ↓
Actuator / Micrometer
    ↓
Prometheus
    ├──────────────► Grafana
    │
    └──────────────► Alertmanager
                           ↓
                         Slack
```

AWS metrics:

```text
AWS / CloudWatch
       ↓
      YACE
       ↓
   Prometheus
       ↓
    Grafana
```

---

# 🧠 APA — AFM Platform Assistant

**APA = AFM Platform Assistant**

APA was introduced as the final evolution of the platform.

It provides a natural-language interface over:

1. Static AFM engineering knowledge
2. Current Kubernetes state
3. Current AWS infrastructure state

The most important architectural principle is:

> **APA is strictly read-only.**

APA is an information and analysis assistant, not an infrastructure remediation engine.

---

# 🚫 APA Safety Boundary

APA can:

- Retrieve AFM documentation
- Retrieve runbooks
- Retrieve troubleshooting knowledge
- Inspect Kubernetes state
- Inspect AWS state
- Combine retrieved evidence
- Explain current platform conditions
- Assist with operational troubleshooting

APA cannot:

- Apply Terraform
- Patch Kubernetes resources
- Delete Kubernetes resources
- Delete AWS resources
- Restart workloads
- Scale workloads
- Modify AWS infrastructure
- Sync Argo CD
- Execute infrastructure-changing commands
- Perform autonomous remediation

This boundary is deliberate.

---

# 🧠 APA Architecture

```text
                         User Question
                              │
                              ▼
                         Streamlit UI
                              │
                              ▼
                         Query Router
                         /           \
                        /             \
                   STATIC           DYNAMIC
                     │                 │
                     ▼                 ├──────────────┐
                 RAG / KB              │              │
                     │                 ▼              ▼
                 ChromaDB         Kubernetes        AWS
                     │                 │              │
                     │              K8s API         Boto3
                     │                 │              │
                     └────────┬────────┴──────────────┘
                              ▼
                       Retrieved Evidence
                              │
                              ▼
                         OpenAI API
                              │
                              ▼
                         Grounded Answer
```

The router decides whether the question requires:

- static project knowledge
- current Kubernetes state
- current AWS state
- or a combination of retrieved evidence

---

# 📚 Static RAG

```text
AFM Documentation
       ↓
Document Processing
       ↓
Chunking
       ↓
Embeddings
       ↓
ChromaDB
       ↓
Semantic Retrieval
       ↓
Relevant Context
       ↓
OpenAI API
       ↓
Grounded Answer
```

The knowledge base contains:

- Architecture
- Repositories
- AWS infrastructure
- Kubernetes
- GitOps
- Terraform
- CI/CD
- Security
- Observability
- Troubleshooting
- Operational runbooks

The knowledge base is maintained separately from the APA application so engineering documentation can evolve independently from the runtime.
# ☁️ Dynamic AWS Access

APA uses Boto3 for approved read-only AWS operations.

```text
APA
 ↓
Boto3
 ↓
AWS APIs
 ↓
Current Infrastructure State
```

Supported information can include:

- EKS
- EC2
- ALB
- RDS
- VPC
- Subnets
- Route tables
- Internet Gateway
- NAT infrastructure
- Security groups

The AWS API surface is deliberately restricted.

---

# ☸️ Dynamic Kubernetes Access

```text
APA
 ↓
Read-only Kubernetes Client
 ↓
Kubernetes API
 ↓
Current EKS State
```

This allows APA to answer questions about the current cluster instead of relying only on stale documentation.

---

# 🧬 APA Evolution

## Stage 1 — Local Static APA

The initial implementation was local:

```text
Streamlit
   ↓
RAG
   ↓
Embeddings
   ↓
ChromaDB
   ↓
Ollama
   ↓
Qwen
   ↓
Answer
```

This stage established the RAG and vector-retrieval foundation.

## Stage 2 — Dynamic APA on EKS

The final implementation evolved into:

```text
Streamlit
   ↓
Query Router
   ├── Static RAG → ChromaDB
   ├── Kubernetes → K8s API
   └── AWS → Boto3
              ↓
       Retrieved Evidence
              ↓
         OpenAI API
              ↓
          GPT-4o-mini
              ↓
       Grounded Response
```

The final APA is therefore not simply a chatbot.

It is a **read-only platform information and analysis layer**.

---

# 🔄 APA vs Existing Operations Tools

APA does not replace the platform's operational systems.

| Component | Primary responsibility |
|---|---|
| Kubernetes | Workload orchestration |
| Argo CD | GitOps reconciliation/deployment |
| Prometheus | Metrics and alert evaluation |
| Grafana | Visualization |
| Alertmanager | Alert routing |
| Slack | Operational notification |
| AWS | Infrastructure platform |
| APA | Natural-language retrieval and explanation |

Example:

```text
Prometheus detects condition
          ↓
Alertmanager routes alert
          ↓
Slack notifies engineer
          ↓
Engineer asks APA for context
          ↓
APA retrieves runbook + current state
          ↓
APA explains the condition
```

APA is therefore an **additional engineering interface**, not a replacement for the underlying operational systems.

---

# 🧰 Technology Stack, Tools & AWS Services

The platform combines application engineering, AWS infrastructure, Kubernetes, CI/CD, GitOps, DevSecOps, observability and AI-assisted read-only operations.

## Application stack

| Technology | Purpose |
|---|---|
| Java 17 | Application runtime |
| Spring Boot 3.3.11 | Application framework and REST services |
| Spring Security | Authentication/security integration |
| Spring Data JPA / Hibernate | Relational persistence |
| PostgreSQL | Application database |
| JWT | Authentication token model |
| BCrypt | Password hashing |
| Maven | Build and dependency management |
| Executable JAR | Container-ready application artifact |
| Docker | Application packaging |

## AWS services

| AWS service | Purpose in AFM v3 |
|---|---|
| Amazon VPC | Network boundary and subnet architecture |
| Amazon EC2 | EKS worker capacity and supporting infrastructure |
| Amazon EKS | Managed Kubernetes control plane |
| Amazon RDS PostgreSQL | Managed relational database |
| Amazon ECR | Container image registry |
| Amazon S3 | Terraform remote state and ALB log storage |
| AWS Secrets Manager | Sensitive secret storage |
| IAM | AWS authorization |
| EKS Pod Identity | APA workload-to-IAM identity |
| IRSA | AFM authentication workload identity |
| Route 53 | DNS |
| ACM | TLS certificate management |
| Application Load Balancer | Public HTTP/HTTPS ingress |
| CloudWatch | AWS infrastructure telemetry |

## Infrastructure and platform tools

| Tool | Purpose |
|---|---|
| Terraform | Infrastructure as Code |
| Terraform Modules | Reusable infrastructure components |
| GitLab | Source control and CI/CD |
| Docker | Container build |
| Kubernetes | Workload orchestration |
| Argo CD | GitOps reconciliation |
| AWS Load Balancer Controller | Kubernetes-to-ALB integration |
| External Secrets | AWS Secrets Manager to Kubernetes integration |

## Security tools

| Tool | Purpose |
|---|---|
| SonarQube | Static code quality/security analysis |
| Trivy | Container vulnerability scanning |
| Trivy IaC | Terraform configuration scanning |
| OWASP ZAP | Dynamic application security testing |
| IAM | AWS access control |
| Kubernetes RBAC | Kubernetes access control |
| IRSA | Workload-specific AWS credentials |
| EKS Pod Identity | Workload-specific AWS access |
| AWS Secrets Manager | Managed secret storage |

## Observability and SRE tools

| Tool | Purpose |
|---|---|
| Spring Boot Actuator | Application health and metrics |
| Micrometer | Application metrics instrumentation |
| Prometheus | Metrics collection and alert-rule evaluation |
| Grafana | Dashboards and SRE views |
| Alertmanager | Alert routing |
| YACE | CloudWatch metrics into Prometheus |
| CloudWatch | AWS telemetry |
| Slack | Operational notifications |

## AI / platform operations

| Technology | Purpose |
|---|---|
| Python | APA runtime |
| Streamlit | APA user interface |
| RAG | Project-specific knowledge retrieval |
| Embeddings | Semantic document retrieval |
| ChromaDB | Vector storage |
| OpenAI API | LLM inference |
| GPT-4o-mini | Cost-conscious production APA model |
| Kubernetes API | Current EKS read-only state |
| Boto3 | Current AWS read-only state |
| Ollama | Local model development |
| Qwen | Initial local APA model |

# 🗂️ Repository Ecosystem

AFM v3 uses separate repositories for infrastructure, Kubernetes desired state, application services and AI components. This separation keeps infrastructure, deployment configuration, application code and AI knowledge independently versioned.

```text
AFM v3 Portfolio
│
├── afm-infra
│      └── Terraform / AWS / EKS
│
├── afm-gitops
│      └── Kubernetes desired state / Argo CD
│
├── afm-auth-service
│      └── Authentication / JWT / persistence
│
├── afm-login-service
│      └── Login service
│
├── afm-registration-service
│      └── Registration service
│
├── afm-frontend-ui
│      └── UI / Reverse Proxy
│
├── APA application
│      └── Streamlit / RAG / dynamic read-only access
│
└── APA knowledge base
       └── AFM documentation / RAG content

```

| Repository / Component | Responsibility |
|---|---|
| `afm-infra` | Terraform-managed AWS/EKS infrastructure |
| `afm-gitops` | Kubernetes desired state and Argo CD configuration |
| `afm-auth-service` | Core identity/authentication service |
| `afm-login-service` | Login gateway |
| `afm-registration-service` | Registration gateway |
| `afm-frontend-ui` | Frontend and Reverse Proxy |
| APA application | Streamlit application, routing, RAG and dynamic integrations |
| APA knowledge base | Static AFM knowledge and documentation |

---

# 🔄 CI/CD Separation

The platform deliberately separates different delivery concerns.

## Application CI/CD

Builds and validates application artifacts.

```text
Source
 ↓
Maven
 ↓
SonarQube
 ↓
Docker
 ↓
Trivy
 ↓
ECR
 ↓
GitOps Update
```

## Infrastructure CI/CD

Manages AWS infrastructure.

```text
Terraform
 ↓
Validation
 ↓
Trivy IaC
 ↓
Plan
 ↓
Controlled Apply
```

## GitOps

Manages Kubernetes desired state.

```text
Git
 ↓
Argo CD
 ↓
EKS
```

## APA

Uses the same platform delivery principles while maintaining a separate AI workload and knowledge lifecycle.

---

# 🔁 Complete CI/CD + Infrastructure + GitOps Lifecycle

AFM v3 separates three related delivery planes: **application CI/CD**, **infrastructure CI/CD**, and **Kubernetes GitOps**.

## Application CI/CD lifecycle

```text
Developer Commit
      ↓
GitLab Pipeline
      ↓
Pre-cleanup
      ↓
Maven Build
      ↓
Unit Tests
      ↓
SonarQube Analysis
      ↓
Docker Image Build
      ↓
Trivy Container Scan
      ↓
Amazon ECR
      ↓
GitOps Repository Update
      ↓
Argo CD Reconciliation
      ↓
Amazon EKS
      ↓
OWASP ZAP Baseline Scan
      ↓
ZAP Quality Gate
```

Each stage has a distinct responsibility:

| Stage | Responsibility | Output |
|---|---|---|
| Pre-cleanup | Prepare the runner/workspace | Clean pipeline execution context |
| Maven | Compile and package Java application | Executable JAR |
| Unit tests | Validate application behavior | Test results |
| SonarQube | Static quality/security analysis | Analysis results |
| Docker | Package the application | Container image |
| Trivy | Scan image vulnerabilities | Security report / gate |
| ECR | Store immutable container artifact | Registry image |
| GitOps update | Update desired deployment state | Git commit |
| Argo CD | Reconcile Git state to EKS | Running workload |
| ZAP | Test the deployed HTTP surface | HTML/JSON reports |
| Quality gate | Apply configurable vulnerability thresholds | Pass/fail decision |

## Infrastructure CI/CD lifecycle

```text
Terraform Source
      ↓
GitLab Infrastructure Pipeline
      ↓
Terraform Format / Validation
      ↓
Trivy IaC
      ↓
Terraform Plan
      ↓
Controlled Apply
      ↓
AWS
      ↓
VPC / EKS / RDS / IAM / ECR / ALB / Secrets
```

The infrastructure pipeline treats Terraform as the source of truth for AWS resources and performs security validation before provisioning.

## GitOps reconciliation lifecycle

```text
Application Repository
      ↓
CI builds artifact
      ↓
Image published to ECR
      ↓
GitOps repository receives desired image/version
      ↓
Argo CD detects Git change
      ↓
Argo CD compares desired vs live state
      ↓
Argo CD synchronizes Kubernetes resources
      ↓
EKS runs the declared workload
      ↓
Prometheus / Grafana observe the result
```

This creates a clear separation:

- **CI** produces and validates artifacts.
- **Git** stores the desired Kubernetes state.
- **Argo CD** performs reconciliation.
- **Kubernetes** runs workloads.
- **Observability** verifies runtime behavior.
- **APA** can explain current state without modifying it.

## Security lifecycle across delivery

```text
Source
  ↓
SonarQube
  ↓
Container Build
  ↓
Trivy Container Scan
  ↓
ECR
  ↓
Deployment
  ↓
OWASP ZAP
  ↓
Runtime Monitoring
```

Infrastructure follows a parallel path:

```text
Terraform
  ↓
Trivy IaC
  ↓
Plan
  ↓
Controlled Apply
  ↓
AWS
```

This creates multiple validation points instead of relying on a single security scanner.

---

# 🧭 Operational Lifecycle / SRE Workflow

AFM v3 treats observability as an operational feedback loop rather than a collection of dashboards.

```text
┌───────────────┐
│    Deploy     │
└───────┬───────┘
        ↓
┌───────────────┐
│    Observe    │
│ Prometheus /  │
│ Grafana / AWS │
└───────┬───────┘
        ↓
┌───────────────┐
│    Detect     │
│ PrometheusRule│
└───────┬───────┘
        ↓
┌───────────────┐
│     Alert     │
│ Alertmanager  │
│    → Slack    │
└───────┬───────┘
        ↓
┌───────────────┐
│  Investigate  │
│ K8s / AWS /   │
│ Grafana / Logs│
└───────┬───────┘
        ↓
┌───────────────┐
│    Explain    │
│      APA      │
└───────┬───────┘
        ↓
┌───────────────┐
│   Remediate   │
│ Engineer-led  │
└───────┬───────┘
        ↓
┌───────────────┐
│    Verify     │
│ Metrics / SLO │
└───────────────┘
```

### Observability responsibilities

| Layer | Responsibility |
|---|---|
| Spring Boot Actuator / Micrometer | Application metrics |
| Prometheus | Metrics collection and rule evaluation |
| Grafana | Operational visualization |
| Alertmanager | Alert grouping and routing |
| Slack | Engineer notification |
| YACE | CloudWatch-to-Prometheus integration |
| CloudWatch | AWS service telemetry |
| APA | Read-only context retrieval and explanation |
| Engineer | Remediation and change execution |

### SRE signals represented in the platform

- Request rate
- Success rate
- 4xx rate
- 5xx rate
- Error rate
- Latency
- p95 latency
- p99 latency
- Service availability
- SLA-oriented views
- Error-budget-oriented views
- Kubernetes workload health
- EKS node health
- RDS health and resource metrics
- ALB target and response metrics

The operational model is intentionally **human-controlled**: automation detects and informs, while infrastructure-changing actions remain under engineer control.

# 🚧 Major Engineering Challenges

AFM v3 evolved through practical engineering problems.

## 1. EKS Pod Capacity

The selected cost-conscious worker capacity created pod/IP pressure.

Resolution:

- VPC CNI Prefix Delegation
- Custom `max-pods`
- Launch Template configuration
- Additional node capacity
- APA workload separation

---

## 2. EKS Provisioning / Dependency Timing

Cluster-dependent resources and addons introduced provisioning-order challenges.

The project required careful dependency and lifecycle handling between:

```text
Terraform
   ↓
EKS Cluster
   ↓
Cluster-dependent resources
   ↓
EKS Addons
```

This became an important infrastructure-engineering lesson around dependency ordering and AWS/EKS initialization behavior.

Detailed implementation evidence belongs in the infrastructure decision/incident documentation.

---

## 3. Prometheus Operator CRDs

Prometheus Operator CRDs created Argo CD synchronization challenges.

The observability deployment was adjusted to control CRD installation and use compatible Argo CD synchronization behavior.

This demonstrates that GitOps does not eliminate Kubernetes lifecycle problems; it changes how they must be managed.

---

## 4. Secrets and Workload Identity

The project evolved from application-level secret handling toward:

```text
AWS Secrets Manager
        ↓
External Secrets
        ↓
Kubernetes
```

and workload identity through:

```text
AFM Auth → IRSA
APA      → EKS Pod Identity
```

This reduced dependence on long-lived credentials.

---

## 5. Observability Evolution

The observability stack evolved from basic application monitoring into:

```text
Application Metrics
        +
Kubernetes Metrics
        +
AWS / RDS Metrics
        +
ALB Metrics
        ↓
Prometheus
        ↓
Grafana
        +
Alertmanager
        ↓
Slack
```

The result is an operations-focused monitoring layer rather than simply a dashboard installation.

---

# 📈 AFM v2 → AFM v3 Evolution

One of the most important architectural changes was the move from manually operated deployment toward GitOps.

### Earlier model

```text
Developer
   ↓
Manual kubectl operations
   ↓
Kubernetes
```

### AFM v3

```text
Developer
   ↓
GitLab CI/CD
   ↓
Build + Security
   ↓
ECR
   ↓
GitOps Repository
   ↓
Argo CD
   ↓
EKS
```

This changed the platform from an application that could be deployed on Kubernetes into a more complete **GitOps-operated platform**.

---

# 🧬 Overall Platform Evolution

```text
                 AFM EVOLUTION
                      │
                      ▼
          Application / Microservices
                      │
                      ▼
                GitLab CI/CD
                      │
                      ▼
                 Docker/ECR
                      │
                      ▼
              Terraform / AWS
                      │
                      ▼
                  Amazon EKS
                      │
                      ▼
                 RDS PostgreSQL
                      │
                      ▼
              HTTPS / ALB / ACM
                      │
                      ▼
                DevSecOps
                      │
                      ▼
                Argo CD GitOps
                      │
                      ▼
             Observability / SRE
                      │
                      ▼
              Static APA / RAG
                      │
                      ▼
          Dynamic APA on EKS
                      │
                      ▼
       AWS + Kubernetes Read-only
```

---

# 📐 Architecture Principles

## Infrastructure as Code

AWS infrastructure is managed through Terraform.

## GitOps

Kubernetes desired state is maintained in Git and reconciled by Argo CD.

## Least Privilege

IAM and Kubernetes RBAC restrict workload access.

## Secure Secrets

Sensitive values are managed through AWS Secrets Manager rather than hard-coded into source code or images.

## Workload Identity

AWS access is provided through workload identity rather than long-lived access keys.

## Shift-Left Security

Security validation is integrated into application and infrastructure delivery.

## Immutable Artifacts

Container images are built and stored in Amazon ECR.

## Centralized Observability

Prometheus and Grafana provide centralized metrics and visualization.

## Cost Awareness

The development environment is intentionally cost-conscious and can be destroyed when not required.

## Read-Only AI Operations

APA is intentionally prevented from modifying infrastructure or workloads.

---

# 📸 Portfolio Evidence

The repository provides visual and operational evidence for the AFM v3 platform.

## Architecture

`diagrams/`

Recommended diagrams:

- Overall AWS architecture
- AFM application architecture
- AFM authentication and registration flow
- Terraform lifecycle
- CI/CD architecture
- GitOps flow
- Security architecture
- Observability architecture
- APA architecture
- APA static/dynamic workflow

## Pipeline Evidence

`pipeline-docs/`

Recommended evidence:

- Infrastructure pipeline
- Application pipeline
- Observability pipeline
- APA pipeline
- Security scanning stages
- GitOps deployment flow

## Screenshots

`screenshots/afm-project/`

Recommended evidence:

- AWS infrastructure
- EKS
- GitLab pipelines
- Argo CD
- Prometheus
- Grafana
- Alertmanager
- Slack alerts
- SonarQube
- Trivy
- OWASP ZAP
- APA UI
- APA Kubernetes read-only response
- APA AWS read-only response

---

# ⚠️ Known Limitations

AFM v3 is a **production-inspired portfolio platform**, not a production deployment or production financial system.

Current limitations include:

- Refresh-token rotation/revocation is not implemented.
- Proxy services do not currently implement advanced resilience patterns such as circuit breakers.
- Blue-Green switching is controlled rather than fully automated progressive delivery.
- Internal service-to-service traffic currently uses HTTP.
- mTLS/service mesh is not implemented.
- Distributed tracing is not currently implemented.
- The infrastructure is intentionally cost-conscious rather than a multi-AZ production architecture.
- APA is strictly read-only.
- APA does not perform autonomous remediation.

These limitations are intentionally documented rather than hidden.

---

# 🛣️ Future Roadmap

Potential future enhancements include:

- OAuth2 / OIDC
- Refresh-token rotation and revocation
- OpenTelemetry distributed tracing
- Automated Blue-Green evaluation
- Progressive delivery based on Prometheus metrics
- Service mesh and mTLS
- Additional resilience patterns
- Expanded SRE automation
- Additional read-only APA tools
- Additional AI-assisted operational workflows while preserving the read-only security boundary

---
# 🛣️ Future Enhancements & Production Evolution

AFM v3 is intentionally scoped as a production-inspired portfolio platform.
The following capabilities represent possible future evolution rather than
current project functionality.

### Application & Security

- OAuth2 / OIDC integration
- Refresh-token rotation and revocation
- Additional service-to-service security
- mTLS where appropriate

### Platform & SRE

- OpenTelemetry distributed tracing
- Automated Blue-Green validation
- Progressive delivery based on Prometheus metrics
- Service mesh where justified
- Additional resilience patterns
- Expanded SRE automation

### AI-Assisted Operations

- Additional read-only APA tools
- Broader Kubernetes and AWS investigation capabilities
- Correlation of metrics, events and platform state
- Additional AI-assisted operational analysis while preserving the
  read-only security boundary

### Production Hardening

A production-oriented deployment would additionally consider:

- Multi-AZ EKS worker capacity
- Private EKS API access or tightly restricted public CIDRs
- EKS secrets encryption with AWS KMS
- Higher availability for supporting platform components
- Stronger disaster-recovery and backup strategy
- Formal SLO/SLI governance
- Centralized audit and security monitoring

---
# 🎓 Skills Demonstrated

## Cloud / Platform Engineering

`AWS` · `VPC` · `EC2` · `EKS` · `RDS` · `ECR` · `S3` · `ALB` · `ACM` · `Route 53` · `Secrets Manager` · `IAM`

## Infrastructure as Code

`Terraform` · `Terraform Modules` · `Remote State` · `S3 Native Locking` · `CI-driven Infrastructure`

## DevOps / GitOps

`GitLab CI/CD` · `Docker` · `Amazon ECR` · `Kubernetes` · `Argo CD` · `GitOps`

## DevSecOps

`SonarQube` · `Trivy` · `Trivy IaC` · `OWASP ZAP` · `IAM` · `IRSA` · `EKS Pod Identity` · `Kubernetes RBAC` · `AWS Secrets Manager`

## Observability / SRE

`Prometheus` · `Grafana` · `Alertmanager` · `YACE` · `CloudWatch` · `Micrometer` · `Spring Boot Actuator` · `Slack`

## Application Engineering

`Java 17` · `Spring Boot 3.3.11` · `Spring Security` · `JPA/Hibernate` · `PostgreSQL` · `JWT` · `BCrypt` · `Maven`

## AI / Platform Operations

`Python` · `Streamlit` · `RAG` · `Embeddings` · `ChromaDB` · `OpenAI API` · `GPT-4o-mini` · `Kubernetes API` · `Boto3` · `Ollama` · `Qwen`

---

# 📚 Portfolio Documentation Structure

The portfolio repository separates project explanation, visual architecture,
implementation evidence and detailed technical documentation.

```text
AFM v3 Portfolio
│
├── README.md
│   └── Project overview and engineering story
│
├── diagrams/
│   └── Architecture and workflow diagrams
│
├── pipeline-docs/
│   └── Detailed CI/CD, infrastructure and APA documentation
│
├── Project-documents/
│   └── Detailed AFM v3 technical project report
│
└── screenshots/
    └── Runtime, pipeline, AWS, Kubernetes, GitOps,
        observability, security and APA evidence
```

The detailed documents answer the deeper:

> **What implementation rationale, alternatives, failure analysis and accepted constraints are documented?**

The root README answers:

> **What is AFM v3, how did it evolve, and what engineering capability does it demonstrate?**

---

# 🏁 Final Project Summary

AFM v3 evolved from a simple application/microservice project into a complete cloud-native **DevOps, DevSecOps, GitOps and platform engineering environment**.

```text
Application Engineering
        +
Terraform / AWS
        +
Amazon EKS
        +
GitLab CI/CD
        +
Docker / ECR
        +
DevSecOps
        +
Argo CD / GitOps
        +
Prometheus / Grafana
        +
SRE Practices
        +
AI / RAG
        +
Read-only Platform Operations
```

The platform demonstrates an end-to-end workflow:

1. Application source is maintained in Git.
2. CI pipelines build, test and scan application code.
3. Docker images are built and published to ECR.
4. Terraform provisions the AWS platform.
5. Kubernetes desired state is maintained in the GitOps repository.
6. Argo CD reconciles that desired state into EKS.
7. Prometheus, Grafana, Alertmanager and YACE provide operational visibility.
8. Security controls run across application, infrastructure and deployed workload stages.
9. APA provides a natural-language, read-only interface over project knowledge and current AWS/Kubernetes state.

The reference application makes the platform concrete, but the primary engineering focus is the **platform lifecycle and the systems around it**.

---

## Documentation Metadata

- **Project:** AFM v3 — GitOps DevSecOps Cloud Platform
- **AFM meaning:** Application / Feature / Microservice
- **Reference/demo application:** AFM Bank
- **Platform version:** `3.0.0`
- **Current phase:** AFM v3 + read-only Dynamic APA
- **Domain:** `afmcloud.in`
- **Documentation:** Portfolio Root README
- **Last reviewed:** August 2026

# 🧭 Engineering Decisions & Trade-offs

The preceding sections describe the **actual AFM v3 project**: its architecture, application components, infrastructure, CI/CD, GitOps process, security, observability, APA implementation, operational model, challenges, limitations and roadmap.

This section intentionally comes **after the project description**.

It contains the deeper engineering rationale: why a technology or pattern was selected, alternatives considered, and trade-offs accepted.

---

## 🔐 Why Does Only `afm-auth-service` Own the Database?

The authentication service is the owner of identity-related persistence.

The other services do not directly connect to PostgreSQL.

```text
Login Service ───────┐
                     │
Registration Service ┤
                     ▼
                Auth Service
                     │
                     ▼
                PostgreSQL
```

This was chosen to avoid database coupling between multiple services.

### Benefits

- Clear data ownership
- Centralized persistence logic
- Reduced schema coupling
- Easier evolution of authentication data
- Smaller database security surface

### Trade-off

The authentication service becomes a central dependency.

For this reference application, this provides a useful service boundary without creating unnecessary microservice fragmentation.

---

## 🖥️ Why a Spring Boot UI/Reverse Proxy Instead of React?

React could have been used.

A React SPA could have been used. The selected UI/Reverse Proxy approach keeps the application intentionally small so that the project can focus on backend, cloud, DevOps and platform engineering.

A Spring Boot-based UI/Reverse Proxy provides:

- A realistic web application
- A public ingress path
- Backend proxy capabilities
- A smaller frontend technology footprint
- Less infrastructure/application complexity

### Trade-off

A React/SPA architecture would provide a richer frontend development model and stronger separation between frontend and backend.

The chosen approach keeps the project focused on platform engineering.

---

## ☕ Application Technology Decisions

| Technology | Why selected | Trade-off |
|---|---|---|
| Java 17 | LTS runtime and modern enterprise baseline | Higher resource footprint than some lightweight runtimes |
| Spring Boot | Mature enterprise Java framework with REST, security, persistence and Actuator support | More framework/runtime overhead |
| Spring Security | Standard security integration for Spring applications | Requires framework configuration and security knowledge |
| Spring Data JPA / Hibernate | Relational persistence abstraction | ORM complexity and potential query/performance issues |
| PostgreSQL | Mature relational database suitable for identity/application data | More operational complexity than a local embedded database |
| JWT | Stateless authentication mechanism suitable for distributed services | Token revocation is more difficult without additional infrastructure |
| BCrypt | Password hashing designed for password storage | Computationally more expensive by design |
| Maven | Conventional Java build lifecycle and strong Spring integration | More verbose/conventional than some alternatives |
| Docker | Consistent application packaging and runtime | Adds image lifecycle and security responsibilities |
| Executable JAR | Self-contained application artifact suited to containers | Less appropriate for environments standardized around external application servers |

---

## 🏗️ Why AWS?

AWS was selected because the project is intended to demonstrate practical cloud/platform engineering using managed cloud primitives.

### Benefits

- Mature managed Kubernetes through EKS
- Native IAM integration
- Managed database through RDS
- Managed container registry through ECR
- Integrated networking
- Managed TLS
- Cloud-native observability integrations

### Trade-off

The platform is intentionally AWS-oriented and therefore carries cloud-provider dependency and AWS-specific operational knowledge requirements.

### Design note

AWS is the chosen cloud implementation, so the platform is intentionally AWS-oriented rather than cloud-neutral. This allows deeper use of native EKS, IAM, RDS, ECR, ALB and CloudWatch integrations.

---

## ☸️ Why Amazon EKS?

EKS was selected instead of running Kubernetes directly on EC2 because the project is intended to demonstrate managed Kubernetes operations.

### Why EKS?

- Managed Kubernetes control plane
- Native AWS IAM integration
- VPC networking
- ECR integration
- Load Balancer Controller integration
- Kubernetes workload orchestration
- Realistic cloud platform model

### Trade-off

EKS introduces:

- AWS dependency
- Additional platform components
- Networking complexity
- Node/pod capacity management
- Additional cost

For this project, the operational learning value justified that complexity.

---

## 🔎 Security Tool Decisions

| Tool | Why used | Trade-off |
|---|---|---|
| SonarQube | Static code quality/security analysis | Adds analysis infrastructure and pipeline time |
| Trivy | Lightweight container vulnerability scanning | Vulnerability results require interpretation and remediation |
| Trivy IaC | Scans Terraform configuration before provisioning | Adds another validation stage |
| OWASP ZAP | Dynamic security validation against deployed application | Runs after deployment and adds pipeline duration |
| AWS Secrets Manager | Managed sensitive secret storage | AWS dependency and additional service cost |
| External Secrets | Bridges AWS-managed secrets into Kubernetes | Adds another controller/component |
| IAM | AWS authorization boundary | IAM policy design can be complex |
| Kubernetes RBAC | Kubernetes authorization | Requires careful permission management |
| BCrypt | Secure password hashing | Computationally expensive by design |


---
## 🔐 EKS Public API Access — Development Environment Decision

The Trivy IaC scan identified the following EKS security findings in the development environment:

| Finding | Severity | Development configuration |
|---|---|---|
| EKS secrets encryption disabled | HIGH | Enabled only when moving toward a production-like environment |
| EKS public API access enabled | CRITICAL | Intentionally enabled for the ephemeral development environment |
| EKS public API CIDR `0.0.0.0/0` | CRITICAL | Intentionally retained for development/test access |

### Why was EKS public access enabled?

The AFM v3 EKS cluster is an **ephemeral development/test platform** that is regularly created and destroyed to control AWS costs.

For this environment, the Kubernetes API endpoint was intentionally configured with public access so that engineers could administer and troubleshoot the cluster using standard Kubernetes tooling such as `kubectl` without introducing additional private-access infrastructure such as a VPN, bastion host or dedicated network connectivity solution.

The development environment prioritizes:

- Rapid cluster creation and destruction
- Simple administrative access
- Portfolio demonstration and testing
- Low infrastructure cost
- Easy troubleshooting during development

The configuration therefore represents an **accepted development-environment trade-off**, not the intended production security posture.

### Why `0.0.0.0/0`?

`0.0.0.0/0` allows the EKS API endpoint to accept connections from any IPv4 source, subject to the other AWS and Kubernetes authentication/authorization controls.

This was retained in the ephemeral development environment to avoid repeatedly updating the EKS API allow-list when the engineer's external IP address changes.

However, this significantly increases the exposure of the Kubernetes API endpoint and was therefore correctly identified by Trivy as a **CRITICAL** infrastructure security finding.

The finding is intentionally documented rather than hidden.

### Production / production-like hardening

For a production or production-like environment, the EKS API endpoint should be hardened by:

1. Restricting public API access to approved corporate/VPN/bastion CIDR ranges, or
2. Using private EKS API access where appropriate.
3. Removing `0.0.0.0/0` from the public access CIDR configuration.
4. Enabling EKS secrets encryption using AWS KMS.
5. Reviewing the resulting access model before enabling the cluster for production workloads.

The important distinction is:

> **The development configuration was intentionally optimized for accessibility and cost, while the production security posture would prioritize restricted API exposure and stronger control-plane protection.**

### DevSecOps lesson

This finding demonstrates why IaC security scanning is useful even when the infrastructure is intentionally configured for a specific development requirement.

Trivy did not simply produce a "failed" result. It identified a security risk, which was then evaluated against:

- Environment purpose
- Operational requirements
- Cost constraints
- Security exposure
- Production hardening requirements

The result is an **explicitly accepted development trade-off with a documented remediation path**, rather than an undocumented security weakness.

---

## 🧪 Why OWASP ZAP Is Post-Deployment

ZAP validates the **running application surface** rather than only source code or container contents.

```text
Internet
   ↓
ALB
   ↓
Frontend
   ↓
Reverse Proxy
   ↓
Application Services
```

The current baseline scan generates HTML and JSON reports and supports configurable quality gates for High, Medium and Low findings.

### Why this approach?

The application must actually be deployed before dynamic behavior can be tested.

### Trade-off

The security scan occurs later in the delivery lifecycle than static analysis and can increase pipeline execution time.

---

## 📈 Why Prometheus?

Prometheus provides:

- Kubernetes-native metrics collection
- PromQL
- Alert rule evaluation
- Integration with Grafana

### Trade-off

Prometheus introduces additional storage, configuration and operational responsibility.

---

## 📊 Why Grafana?

Grafana provides the operational visualization layer.

Dashboards cover:

- AFM services
- Kubernetes workloads
- EKS nodes
- ALB
- RDS PostgreSQL
- SRE metrics
- Platform operations

SRE-oriented dashboards include:

- Request rate
- Success rate
- 4xx
- 5xx
- Error rate
- Latency
- p95
- p99
- SLA-oriented views
- Error-budget-oriented views

---

## 🚨 Why Alertmanager and Slack?

Dashboards require engineers to actively look at them.

Alertmanager provides:

- Alert routing
- Grouping
- Notification handling

Slack provides an operational notification channel.

```text
Metric
  ↓
Prometheus
  ↓
PrometheusRule
  ↓
Alertmanager
  ↓
Slack
  ↓
Engineer
```

---

## ☁️ Why YACE?

CloudWatch already contains AWS infrastructure metrics.

YACE was selected to expose selected CloudWatch metrics through the Prometheus ecosystem.

This allows application, Kubernetes and AWS infrastructure metrics to be viewed together through Grafana.

Examples include:

- RDS CPU utilization
- Database connections
- Free storage
- Freeable memory
- Read/write IOPS
- Read/write latency
- ALB target health
- ALB response/latency metrics

### Trade-off

YACE introduces another observability component, but provides a unified Prometheus/Grafana operational model.

---

## 🤖 Why RAG?

A general-purpose LLM does not inherently know the internal architecture of AFM.

RAG provides project-specific context before generating the response.

### Benefits

- Grounds answers in project documentation
- Reduces dependence on model memory
- Allows AFM-specific terminology
- Makes runbooks searchable
- Separates knowledge from model inference

### Trade-off

RAG adds:

- Document processing
- Embedding generation
- Vector storage
- Retrieval logic
- Knowledge-base maintenance

---

## 🧠 APA Technology Decisions

| Technology | Why selected | Trade-off |
|---|---|---|
| Python | Strong ecosystem for AI/RAG integrations | Different runtime from AFM Java services |
| Streamlit | Fast, lightweight operational UI | Less control than a full frontend framework |
| OpenAI API | Managed LLM inference | API cost and external dependency |
| GPT-4o-mini | Cost/performance appropriate for the portfolio workload | Model capabilities differ from larger models |
| ChromaDB | Lightweight vector store suitable for project-scale RAG | Not intended as a full enterprise-scale vector platform |
| RAG | Grounds answers in AFM-specific knowledge | Requires ingestion and retrieval pipeline |
| Kubernetes API | Current EKS state | Requires strict RBAC design |
| Boto3 | Native AWS SDK access | Requires IAM policy design |
| Query Router | Separates static knowledge from dynamic state | Adds routing logic and testing complexity |
| EKS Pod Identity | AWS workload identity for APA | Requires EKS/IAM configuration |
| Ollama + Qwen | Enabled local/offline development of the initial APA | Local model quality and hardware constraints |

---

## Additional Design Notes

### Why a NAT Instance?

A NAT Gateway is operationally simpler but has a higher baseline cost.

A NAT Instance was selected for the portfolio environment because the project prioritizes cost awareness.

### Trade-off

NAT Instance:

- Lower cost for this use case
- More operational responsibility
- Less resilient than managed NAT Gateway architecture

### Why Route 53?

Provides AWS-native DNS integration.

### Why ACM?

Provides managed certificate lifecycle.

### Why ALB?

Provides an AWS-native HTTP/HTTPS entry point and integrates with Kubernetes through the AWS Load Balancer Controller.

### Trade-off

The implementation is strongly AWS-integrated rather than cloud-neutral.

### Why Terraform?

Manual AWS provisioning would make the platform:

- difficult to reproduce
- difficult to review
- difficult to destroy
- difficult to evolve consistently

Terraform provides:

- Declarative infrastructure
- Version-controlled infrastructure
- Reproducible environments
- Dependency management
- Plan/apply workflow
- Module reuse

### Trade-off

Terraform introduces:

- State management
- Module design
- Dependency handling
- Provider/version management
- Terraform-specific debugging

The trade-off is worthwhile because the platform must be recreated repeatedly.

### Why Prefix Delegation?

It increases available pod IP allocation capacity compared with relying only on individual secondary IP allocation.

### Why custom `max-pods`?

The default Kubernetes pod limit calculation does not necessarily represent the capacity available after the selected networking configuration.

### Trade-off

Increasing scheduling capacity does not magically increase CPU or memory.

The configuration must therefore be matched with actual node resources and workload requirements.



## 💰 Infrastructure Cost and Capacity Decisions

| Decision | Rationale | Trade-off |
|---|---|---|
| Cost-conscious worker nodes | Keep portfolio infrastructure affordable | Lower compute headroom and resilience |
| NAT Instance | Lower baseline cost for the development environment | More operational responsibility and lower resilience than NAT Gateway |
| Ephemeral EKS | Avoid paying for an always-on development cluster | Environment is unavailable when destroyed |
| Separate APA node | Isolate Python/LLM workload from Java/observability workloads | Additional worker cost |
| VPC CNI Prefix Delegation | Increase available pod/IP capacity on cost-conscious nodes | Adds networking configuration complexity |
| Custom `max-pods` | Align Kubernetes scheduling capacity with selected networking configuration | Requires capacity validation against real CPU/memory resources |

## 🌐 AWS Edge and Networking Decisions

| Technology | Why selected | Trade-off |
|---|---|---|
| Route 53 | AWS-native DNS integration | AWS dependency |
| ACM | Managed certificate lifecycle | AWS dependency |
| ALB | AWS-native HTTP/HTTPS entry point and Kubernetes integration | AWS-specific ingress model |
| AWS Load Balancer Controller | Connects Kubernetes ingress resources to ALB | Additional controller lifecycle |
| NAT Instance | Cost-conscious outbound connectivity for the portfolio environment | Less resilient and more operationally involved than NAT Gateway |

## 🔑 Workload Identity Decisions

| Workload | Mechanism | Why selected | Trade-off |
|---|---|---|---|
| `afm-auth-service` | IRSA | Supports AWS access required by authentication/Secrets Manager responsibilities | IAM and service-account configuration |
| APA | EKS Pod Identity | Provides dedicated AWS identity for read-only platform integrations | Requires EKS/IAM configuration |
| APA Kubernetes access | Kubernetes RBAC | Restricts APA to approved read-only resources | Requires careful permission design |

## 🔄 GitOps Decision

| Decision | Rationale | Trade-off |
|---|---|---|
| Argo CD + GitOps | Declarative desired state, reconciliation, drift detection and Git-based deployment history | Additional controller and GitOps repository |
| CI/CD separation | CI produces artifacts while CD reconciles desired state | Requires coordination between application and GitOps repositories |
| Controlled Blue-Green | Clear release/rollback model for authentication service | Requires temporary capacity for two versions and controlled selector changes |

## Consolidated Decision Matrix

| Area | Selected approach | Primary rationale | Main trade-off |
|---|---|---|---|
| Cloud | AWS | Managed cloud services and deep EKS integration | AWS dependency |
| Infrastructure | Terraform | Reproducible, version-controlled infrastructure | State and module complexity |
| Kubernetes | Amazon EKS | Managed Kubernetes with AWS integration | Platform complexity and cost |
| CI | GitLab CI/CD | Pipeline-as-code and integrated delivery workflow | Runner infrastructure |
| Containers | Docker + ECR | Consistent packaging and AWS-native registry | Image lifecycle/security overhead |
| CD | Argo CD GitOps | Declarative desired state and reconciliation | Additional controller/repository |
| Application | Java 17 + Spring Boot | Mature application framework and LTS runtime | Runtime/resource overhead |
| Database | PostgreSQL + RDS | Managed relational persistence | Database/AWS dependency |
| Secrets | Secrets Manager + External Secrets | Managed secret storage and Kubernetes integration | Additional components |
| Identity | IRSA + EKS Pod Identity | Workload-specific AWS access without long-lived keys | IAM/EKS configuration |
| Security | SonarQube + Trivy + ZAP | Multiple layers of application/IaC/runtime validation | Pipeline time and finding triage |
| Observability | Prometheus + Grafana + Alertmanager + YACE | Unified Kubernetes/application/AWS monitoring | Additional monitoring stack |
| AI | RAG + OpenAI + dynamic AWS/Kubernetes reads | Project-grounded, current platform information | API cost and integration complexity |
| APA security | Strictly read-only | Minimize operational blast radius | No autonomous remediation |
| Infrastructure cost | Cost-conscious / ephemeral EKS | Suitable for portfolio development | Lower availability and resilience |
