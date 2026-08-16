# AFM v3 — GitOps DevSecOps Banking Platform

> **Automated Financial Management (AFM)** — a production-inspired cloud-native platform demonstrating AWS, Terraform, Amazon EKS, GitLab CI/CD, DevSecOps, Argo CD GitOps, observability, and AI-assisted read-only platform operations.

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

## 📌 Project Snapshot

| Attribute | Details |
|---|---|
| Platform | AFM — Automated Financial Management |
| Version | `3.0.0` |
| Domain | `afmcloud.in` |
| Cloud | AWS |
| Kubernetes | Amazon EKS |
| Application Runtime | Java 17 |
| Framework | Spring Boot 3.3.11 |
| CI/CD | GitLab CI/CD |
| Container Registry | Amazon ECR |
| Deployment | Argo CD / GitOps |
| IaC | Terraform |
| Security | SonarQube, Trivy, Trivy IaC, OWASP ZAP |
| Observability | Prometheus, Grafana, Alertmanager, YACE, CloudWatch |
| AI Operations | APA — AFM Platform Assistant |
| LLM | OpenAI API / GPT-4o-mini |
| Vector Store | ChromaDB |
| UI | Streamlit |

---

# 🚀 Overview

The **Automated Financial Management (AFM) v3** platform is a production-inspired cloud-native microservices platform built to demonstrate an end-to-end DevOps and platform engineering lifecycle on AWS.

The platform brings together:

- Infrastructure as Code with Terraform
- AWS networking and managed services
- Amazon EKS and Kubernetes
- GitLab CI/CD
- Docker and Amazon ECR
- Argo CD GitOps
- Application and infrastructure security scanning
- Route 53 and ACM-managed HTTPS
- AWS Secrets Manager
- External Secrets
- Kubernetes RBAC
- EKS Pod Identity
- Prometheus and Grafana
- Alertmanager and Slack notifications
- YACE for AWS/CloudWatch metrics
- SRE-oriented dashboards
- Cost-conscious infrastructure design
- **APA — a strictly read-only AI Platform Assistant**

The project is intentionally **production-inspired rather than a production banking system**. The architecture demonstrates real-world engineering patterns while operating within practical portfolio-project constraints such as limited infrastructure budget and ephemeral development infrastructure.

---


## 📑 Table of Contents

- [Project Snapshot](#-project-snapshot)
- [Overview](#-overview)
- [Project Objectives](#-project-objectives)
- [End-to-End Platform Lifecycle](#-end-to-end-platform-lifecycle)
- [Platform Architecture](#-platform-architecture)
- [AWS Platform Architecture](#-aws-platform-architecture)
- [DNS, HTTPS and TLS](#-dns-https-and-tls)
- [Infrastructure as Code — Terraform](#-infrastructure-as-code--terraform)
- [Amazon EKS Platform](#-amazon-eks-platform)
- [Application Microservices](#-application-microservices)
- [DevSecOps Architecture](#-devsecops-architecture)
- [Blue-Green Deployment](#-blue-green-deployment--authentication-service)
- [GitOps with Argo CD](#-gitops-with-argo-cd)
- [Observability](#-observability)
- [AI Platform Operations — APA](#-ai-platform-operations--apa)
- [APA Architecture](#-apa-architecture)
- [Operational Challenges](#-operational-challenges-and-engineering-decisions)
- [Portfolio Evidence](#-portfolio-evidence)
- [Known Limitations](#-known-limitations)
- [Future Roadmap](#-future-roadmap)
- [Skills Demonstrated](#-skills-demonstrated)
- [Final Takeaway](#-final-takeaway)


# 🎯 Project Objectives

AFM v3 was designed to demonstrate the ability to:

1. Provision AWS infrastructure reproducibly using Terraform.
2. Deploy and operate workloads on Amazon EKS.
3. Build application containers through GitLab CI/CD.
4. Scan application code, container images and infrastructure configuration.
5. Store immutable application images in Amazon ECR.
6. Deploy Kubernetes workloads through GitOps with Argo CD.
7. Secure application secrets using AWS Secrets Manager and workload identity.
8. Expose the application through Route 53, ACM and an AWS Application Load Balancer.
9. Monitor application, Kubernetes and AWS infrastructure.
10. Deliver operational alerts through Alertmanager and Slack.
11. Handle Kubernetes pod-capacity constraints on cost-conscious worker nodes.
12. Provide engineers with a natural-language, read-only interface to platform knowledge and current infrastructure state through APA.

---

# 🔄 End-to-End Platform Lifecycle

```text
                         Developer
                            │
                            ▼
                         GitLab
                            │
             ┌──────────────┴──────────────┐
             │                             │
             ▼                             ▼
       Application CI/CD             Infrastructure CI/CD
             │                             │
             ▼                             ▼
      Security Validation             Terraform / Trivy IaC
             │                             │
             ▼                             ▼
        Docker Image                  AWS / EKS Platform
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
             │
      ┌──────┼─────────┐
      ▼      ▼         ▼
   AFM Apps  APA   Observability
               │
        ┌──────┴──────┐
        ▼             ▼
      Static       Dynamic
       RAG       AWS / K8s Read-only
        │             │
        └──────┬──────┘
               ▼
          OpenAI API
               │
               ▼
        AI-grounded response
```

---

# 🏗️ Platform Architecture

AFM uses a **Backend-for-Frontend (BFF)** pattern.

The frontend is the public application entry point. It presents the dashboard and proxies selected API requests to the internal registration and login services. Those gateway services communicate with the core authentication service.

The authentication service owns the persistence and identity-related operations and accesses Amazon RDS PostgreSQL and AWS Secrets Manager.

```text
                         Client / Browser
                                │
                         HTTPS — afmcloud.in
                                │
                                ▼
                    Application Load Balancer
                                │
                         Kubernetes Ingress
                                │
                                ▼
                     afm-frontend-ui
                     BFF / Dashboard
                       │             │
                       │             │
                       ▼             ▼
              Registration         Login
                 Service           Service
                       │             │
                       └──────┬──────┘
                              ▼
                       afm-auth-service
                       Identity / JWT Core
                         │            │
                         ▼            ▼
                  Amazon RDS       AWS Secrets
                   PostgreSQL       Manager
```

### Architectural responsibilities

| Component | Responsibility |
|---|---|
| `afm-frontend-ui` | Customer-facing UI and BFF/proxy layer |
| `afm-registration-service` | Registration gateway/proxy |
| `afm-login-service` | Login/authentication gateway/proxy |
| `afm-auth-service` | Core identity, JWT, persistence and secret access |
| Amazon RDS PostgreSQL | Persistent application data |
| AWS Secrets Manager | Sensitive credentials and JWT-related secrets |
| Amazon EKS | Kubernetes runtime |
| ALB + Ingress | Public HTTPS entry and routing |

---

# ☁️ AWS Platform Architecture

```text
                              Internet
                                  │
                                  ▼
                            Route 53 DNS
                                  │
                                  ▼
                         afmcloud.in / HTTPS
                                  │
                                  ▼
                    AWS Application Load Balancer
                                  │
                         ACM TLS Certificate
                                  │
                                  ▼
                    AWS Load Balancer Controller
                                  │
                                  ▼
                         Kubernetes Ingress
                                  │
                                  ▼
                           Amazon EKS
                  ┌───────────────┴───────────────┐
                  │                               │
             Worker Node                     APA Node
                  │                               │
            AFM / Platform                      APA
             Workloads                        Workload
                  │                               │
                  └───────────────┬───────────────┘
                                  │
                   ┌──────────────┼──────────────┐
                   ▼              ▼              ▼
              Amazon RDS    AWS Secrets       Amazon S3
              PostgreSQL      Manager       State / Logs
```

The platform also uses AWS networking, IAM, ECR, Route 53, ACM, ALB, NAT infrastructure, CloudWatch and other supporting services.

---

# 🌐 DNS, HTTPS and TLS

AFM v3 uses the custom domain:

```text
afmcloud.in
```

Public traffic follows:

```text
Client
  ↓
Route 53
  ↓
HTTPS
  ↓
Application Load Balancer
  ↓
ACM Certificate
  ↓
TLS Termination
  ↓
Kubernetes Ingress
  ↓
ClusterIP Service
  ↓
Application Pod
```

### Security model

- Route 53 provides DNS resolution.
- AWS Certificate Manager provides the public TLS certificate.
- The Application Load Balancer terminates public TLS.
- The AWS Load Balancer Controller manages the ALB from Kubernetes resources.
- Backend services are not directly exposed to the Internet.
- Internal service-to-service communication currently uses HTTP inside the EKS networking boundary.
- Network isolation is provided through private networking, Security Groups, Kubernetes RBAC and IAM/workload identity.
- mTLS is **not** currently implemented because the platform does not deploy a service mesh.

### Future enterprise enhancement

A service mesh such as Istio or Linkerd could be introduced later for:

- mTLS
- Service-to-service identity
- Advanced traffic policies
- Distributed tracing integration
- More granular service observability

---

# 🏗️ Infrastructure as Code — Terraform

Infrastructure is provisioned using **Terraform** following Infrastructure as Code principles.

The Terraform code is organized into reusable modules and environment-specific configurations.

## Major infrastructure areas

- VPC
- Public/private networking
- Route tables
- Internet Gateway
- NAT Instance
- IAM
- EKS
- EKS cluster base
- EKS addons
- ECR
- RDS PostgreSQL
- Route 53
- ACM
- ALB logging
- AWS Secrets Manager
- External Secrets
- EKS Pod Identity

## Terraform state

Terraform remote state is stored in Amazon S3.

The current implementation uses **S3 native state locking**:

```hcl
use_lockfile = true
```

DynamoDB is not used for Terraform state locking in the current implementation.

## Infrastructure lifecycle

```text
Terraform Source
       ↓
GitLab CI/CD
       ↓
Validate
       ↓
Trivy IaC Scan
       ↓
Terraform Plan
       ↓
Approval / Pipeline Control
       ↓
Terraform Apply
       ↓
AWS Platform
```

Infrastructure can also be destroyed through the controlled infrastructure pipeline when the ephemeral development environment is no longer required.

---

# ☸️ Amazon EKS Platform

Amazon EKS is the runtime platform for:

- AFM application workloads
- Kubernetes platform components
- Observability components
- APA

The EKS lifecycle is organized around:

```text
EKS Cluster
    │
    ├── Cluster Base
    │
    └── EKS Addons
```

The platform is designed for cost-conscious development/testing rather than high-availability production workloads.

---

# 📈 EKS Pod Capacity and Networking

Worker-node pod capacity became an important engineering consideration during the project.

AFM v3 uses:

### VPC CNI Prefix Delegation

Prefix delegation increases the available pod IP allocation capacity compared with relying only on individual secondary IP allocation.

### Custom max-pods configuration

The worker-node launch template is configured so that Kubernetes pod scheduling capacity is not limited only by the default instance-type calculation.

Conceptually:

```text
EKS Worker Node
      │
      ├── ENI
      │
      ├── Prefix Delegation
      │
      └── Custom max-pods configuration
                    ↓
             Higher pod capacity
```

This allows the selected cost-conscious EC2 instance type to support more Kubernetes workload pods than its unmodified default pod limit.

---

# 🖥️ Worker-Node and APA Workload Separation

The current platform uses two EKS worker nodes.

The platform/application workloads and APA are separated so that APA does not unnecessarily compete with the primary AFM workload capacity.

```text
                    Amazon EKS
                         │
             ┌───────────┴───────────┐
             │                       │
             ▼                       ▼
        Platform Node            APA Node
             │                       │
      AFM / Platform                 APA
        Workloads                  Workload
```

The dedicated APA capacity is particularly useful because APA adds an additional Python/LLM-oriented workload to the platform.

---

# 🌐 VPC and NAT Architecture

The platform uses a Terraform-managed VPC with public and private networking.

Major components include:

- VPC
- Public subnets
- Private subnets
- Route tables
- Internet Gateway
- NAT Instance
- Security Groups
- EKS networking
- Application Load Balancer

A NAT Instance is used for outbound connectivity from private networking as part of the project's cost-conscious design.

---

# 🧩 Application Microservices

AFM contains four application components.

| Service | Port | Type | Responsibility |
|---|---:|---|---|
| `afm-frontend-ui` | `8080` | Web App / BFF | Dashboard UI and public BFF/proxy entry point |
| `afm-registration-service` | `8081` | REST Proxy | Registration gateway |
| `afm-login-service` | `8082` | REST Proxy | Login/authentication gateway |
| `afm-auth-service` | `8080` | Identity Core | Authentication, JWT, persistence and secret access |

The frontend and backend services are independently containerized and deployed.

---

# 🔐 Application Authentication

The core authentication service provides:

- User registration
- Password hashing using BCrypt
- User persistence
- JWT generation
- JWT validation
- Authentication-related API endpoints
- AWS Secrets Manager integration

The platform currently uses JWT-based authentication.

Refresh-token rotation and explicit token revocation/blacklisting are not part of the current implementation.

---

# 🔌 Unified API Reference

The following routes represent the documented application flow.

## User Registration

```http
POST /api/proxy/register
POST /api/register
POST /api/auth/register

Content-Type: application/json

{
  "username": "johndoe",
  "password": "SecurePassword123!"
}
```

Expected responses include:

- `200 OK` / `201 Created` for successful registration
- `400 Bad Request` for invalid registration input
- `409 Conflict` when the username already exists

## User Login

```http
POST /api/proxy/login
POST /api/login
POST /api/auth/login

Content-Type: application/json

{
  "username": "johndoe",
  "password": "SecurePassword123!"
}
```

Successful authentication returns a JWT and user information.

## Token Validation

```http
POST /api/login/validate
POST /api/auth/validate

Content-Type: application/json

{
  "token": "eyJhbGci..."
}
```

The validation response indicates whether the token is valid and, when valid, provides the associated user information and expiry information.

---

# 🛡️ DevSecOps Architecture

AFM integrates security controls into both application and infrastructure delivery.

## Application pipeline

The application pipeline follows the current seven-stage delivery flow:

```text
1. Pre-cleanup
       ↓
2. Maven Build / Test
       ↓
3. SonarQube Analysis
       ↓
4. Docker Build & Push to ECR
       ↓
5. Trivy Container Scan
       ↓
6. GitOps Update
       ↓
7. OWASP ZAP Scan / Quality Gate
```

The exact pipeline implementation can vary slightly by repository, but the security and delivery controls are consistently applied across the application ecosystem.

## Infrastructure pipeline

Infrastructure security is handled separately:

```text
Terraform Code
      ↓
Validation
      ↓
Trivy IaC
      ↓
Terraform Plan
      ↓
Apply
```

This keeps infrastructure configuration scanning separate from container/image scanning.

---

# 🔎 Security Tooling

| Tool / Control | Purpose |
|---|---|
| SonarQube | Static code analysis / SAST |
| Trivy | Container image vulnerability scanning |
| Trivy IaC | Terraform/IaC security scanning |
| OWASP ZAP | Dynamic application security testing |
| AWS Secrets Manager | Sensitive secret storage |
| External Secrets | Secret synchronization into Kubernetes |
| EKS Pod Identity | AWS workload identity |
| Kubernetes RBAC | Kubernetes authorization |
| IAM | AWS authorization |
| ACM | TLS certificates |
| Security Groups | Network-level access control |
| BCrypt | Password hashing |

---

# 🧪 OWASP ZAP

OWASP ZAP is integrated as a post-deployment security validation stage.

The current ZAP baseline implementation uses the ZAP container and generates HTML/JSON reports.

The scan can be configured with quality-gate thresholds for:

- High findings
- Medium findings
- Low findings

The current architecture scans the publicly reachable application surface through the frontend entry point because the frontend/ALB path is the Internet-facing boundary.

Conceptually:

```text
Internet
   ↓
ALB
   ↓
afm-frontend-ui
   ↓
BFF / Internal APIs
   ↓
AFM Services
```

This avoids exposing backend services simply to perform external security testing.

---

# 🔑 Secrets Management and Workload Identity

Sensitive values are not hard-coded into application source code or container images.

The platform uses:

```text
AWS Secrets Manager
        │
        ▼
External Secrets
        │
        ▼
Kubernetes Secret
        │
        ▼
Application
```

For AWS API access from Kubernetes workloads:

```text
Kubernetes Pod
      ↓
EKS Pod Identity
      ↓
IAM Role
      ↓
AWS API
```

This avoids embedding long-lived AWS access keys in the container.

The authentication service uses AWS Secrets Manager for sensitive database and JWT-related secret material.

---

# 🟢 Blue-Green Deployment — Authentication Service

`afm-auth-service` uses a Blue-Green deployment model.

```text
                    Kubernetes Service
                         │
                         ▼
                  Active Color
                 ┌───────────────┐
                 │ Blue OR Green  │
                 └───────────────┘
                    ▲         ▲
                    │         │
             Active Version  Idle Version
                    │         │
                 ┌───────┐  ┌───────┐
                 │ Blue  │  │ Green │
                 └───────┘  └───────┘
```

### Deployment flow

1. The pipeline determines the currently active color.
2. The new image is deployed to the idle color.
3. The idle workload is validated.
4. Traffic is switched by changing the Kubernetes Service selector through the GitOps workflow.
5. If a rollback is required, the previous Service selector can be restored.

The current traffic-switch operation is **not presented as an automatic production failover mechanism**. The project documents it as a controlled Blue-Green release process with explicit switch/rollback tooling.

This distinction is intentional: the portfolio demonstrates the deployment pattern without claiming production-grade automated release orchestration that is not currently implemented.

---

# 🔄 GitOps with Argo CD

AFM uses a pull-based GitOps deployment model.

Application CI/CD does not directly perform the final Kubernetes deployment.

Instead:

```text
Developer Commit
       ↓
GitLab CI/CD
       ↓
Build / Security
       ↓
Docker Image
       ↓
Amazon ECR
       ↓
GitOps Repository Update
       ↓
Argo CD
       ↓
Amazon EKS
```

Argo CD continuously reconciles the desired Kubernetes state stored in Git with the running cluster.

### GitOps benefits demonstrated

- Declarative Kubernetes configuration
- Version-controlled desired state
- Drift detection
- Automated reconciliation
- Reproducible deployments
- Git-based rollback capability

---

# 📊 Observability

AFM v3 implements centralized application, Kubernetes and AWS observability.

## Application metrics

Spring Boot Actuator and Micrometer expose application metrics.

Prometheus scrapes the application metrics endpoints.

```text
Spring Boot Application
        │
        ▼
Actuator / Micrometer
        │
        ▼
Prometheus
        │
        ├──────────────► Grafana
        │
        └──────────────► Alertmanager
                               │
                               ▼
                             Slack
```

Common endpoints include:

```text
/actuator/health
/actuator/prometheus
```

## Grafana

The platform includes dashboards for:

- AFM application services
- Kubernetes workloads
- EKS nodes
- ALB
- RDS PostgreSQL
- SRE-oriented metrics
- Platform operations

The SRE dashboard includes metrics such as:

- Request rate
- Success rate
- 4xx rate
- 5xx rate
- Error rate
- Latency
- p95/p99 latency
- SLA-oriented views
- Error-budget-oriented views

## AWS metrics through YACE

YACE exports selected CloudWatch metrics into Prometheus.

The current monitoring configuration includes AWS/RDS and ALB-related metrics such as:

- RDS CPU utilization
- RDS database connections
- RDS free storage
- RDS freeable memory
- RDS read/write IOPS
- RDS read/write latency
- ALB target health
- ALB response/latency metrics

This allows AWS infrastructure and Kubernetes/application metrics to be viewed through Grafana.

---

# 🚨 Alerting

Prometheus alert rules are used for application and platform conditions.

Alertmanager handles alert routing.

Slack is used as an operational notification channel.

Conceptually:

```text
Metrics
   ↓
Prometheus
   ↓
PrometheusRule
   ↓
Alertmanager
   ↓
Slack
```

The platform includes custom rules for AFM service health and infrastructure conditions.

---

# 🧱 Kubernetes Architecture

AFM workloads run on Amazon EKS.

The platform uses Kubernetes resources such as:

- Deployments
- Services
- Ingress
- ConfigMaps
- Secrets
- Readiness probes
- Liveness probes
- ReplicaSets generated by Deployments
- Blue-Green workload resources for authentication

Persistent storage is used where required by platform components such as observability; it is **not claimed as a persistence requirement for every AFM microservice**.

Horizontal Pod Autoscaling is **not presented as a core AFM v3 feature** in this README because the current portfolio platform is intentionally focused on cost-conscious, controlled EKS capacity.

---

# 📡 Observability Architecture

```text
AFM Services
     │
     ▼
Spring Boot Actuator
     │
     ▼
Prometheus
     │
     ├───────────────► Grafana
     │
     └───────────────► Alertmanager
                              │
                              ▼
                            Slack

AWS CloudWatch
     │
     ▼
    YACE
     │
     ▼
Prometheus
     │
     ▼
Grafana
```

Platform observability components include:

- Prometheus
- Grafana
- Alertmanager
- Prometheus Operator / kube-prometheus-stack
- YACE
- CloudWatch
- Spring Boot Actuator
- Micrometer
- Custom Prometheus rules
- Slack notifications

---

# 🧰 Technology Stack

## Application

- Java 17
- Spring Boot 3.3.11
- Spring Security
- Spring MVC
- Spring Data JPA / Hibernate
- PostgreSQL
- JWT
- BCrypt
- Maven

## Cloud

- AWS
- Amazon EKS
- Amazon EC2
- Amazon RDS PostgreSQL
- Amazon ECR
- Amazon S3
- AWS Secrets Manager
- IAM
- EKS Pod Identity
- Route 53
- ACM
- Application Load Balancer
- NAT Instance
- CloudWatch

## DevOps / GitOps

- GitLab CI/CD
- Docker
- Terraform
- Argo CD
- Kubernetes

## Security

- SonarQube
- Trivy
- Trivy IaC
- OWASP ZAP
- Kubernetes RBAC
- IAM
- AWS Secrets Manager
- External Secrets

## Observability

- Prometheus
- Grafana
- Alertmanager
- YACE
- CloudWatch
- Slack

## AI / RAG

- Python
- Streamlit
- OpenAI API
- GPT-4o-mini
- RAG
- Embeddings
- ChromaDB
- Kubernetes API
- AWS SDK / Boto3
- Ollama
- Qwen

---

# 🗂️ Repository Ecosystem

AFM v3 separates infrastructure, desired state, application services and AI components.

```text
                         AFM v3
                           │
        ┌──────────────────┼────────────────────┐
        │                  │                    │
        ▼                  ▼                    ▼
    afm-infra          afm-gitops          AFM Services
        │                  │                    │
        ▼                  ▼                    ▼
    Terraform           Argo CD         GitLab CI/CD → ECR
        │                  │                    │
        └──────────────────┼────────────────────┘
                           ▼
                         EKS
                           │
                    ┌──────┴──────┐
                    ▼             ▼
                 AFM Apps        APA
                                  │
                         ┌────────┴────────┐
                         ▼                 ▼
                  APA Application    APA Knowledge Base
```

| Repository | Responsibility |
|---|---|
| `afm-infra` | Terraform AWS/EKS infrastructure |
| `afm-gitops` | Kubernetes desired state and Argo CD configuration |
| `afm-auth-service` | Core authentication and identity service |
| `afm-login-service` | Login gateway |
| `afm-registration-service` | Registration gateway |
| `afm-frontend-ui` | Frontend and BFF |
| APA application repository | Streamlit application, query routing, RAG and dynamic integrations |
| APA knowledge-base repository | Static RAG documentation and knowledge content |

---

# 📁 Project Structure

The portfolio repository contains documentation and evidence around the separate platform repositories.

```text
AFM v3 Portfolio
├── README.md
├── diagrams/
├── pipeline-docs/
├── screenshots/
│   └── afm-project/
└── Project-documents/
```

The application repositories follow their own service-specific structures containing:

```text
src/
scripts/
Dockerfile
.gitlab-ci.yml
```

The infrastructure and GitOps repositories are maintained independently.

---

# 🧠 AI Platform Operations — APA

## AFM Platform Assistant

**APA (AFM Platform Assistant)** is the AI-powered operations layer added to AFM v3.

APA provides a natural-language interface over:

1. AFM's static engineering knowledge.
2. Current Kubernetes information.
3. Current AWS infrastructure information.

The key design principle is:

> **APA is strictly read-only.**

It is an information and analysis assistant, not an infrastructure remediation engine.

### APA can

- Retrieve AFM documentation
- Retrieve relevant runbooks and troubleshooting knowledge
- Inspect current Kubernetes state through approved read-only APIs
- Inspect current AWS state through approved read-only APIs
- Combine retrieved evidence
- Explain current platform conditions
- Help engineers troubleshoot operational questions

### APA cannot

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

This separation is deliberate and forms part of APA's security boundary.

---

# 🧠 APA Architecture

APA combines **static RAG** and **dynamic infrastructure retrieval**.

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
                      ▼                 ├───────────────┐
                 RAG Retrieval          │               │
                      │                 ▼               ▼
                      ▼            Kubernetes        AWS Client
                   ChromaDB           Client              │
                      │                 │                  │
                      │                 ▼                  ▼
                      │              EKS API            AWS APIs
                      │                 │                  │
                      └────────┬────────┴──────────────────┘
                               ▼
                        Retrieved Evidence
                               │
                               ▼
                         Query / Context
                               │
                               ▼
                          OpenAI API
                               │
                               ▼
                          GPT-4o-mini
                               │
                               ▼
                       Grounded Response
```

The router separates questions that can be answered from the knowledge base from questions that require current platform state.

---

# 📚 APA Static RAG

The static knowledge path uses Retrieval-Augmented Generation.

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
GPT-4o-mini
       ↓
Grounded Answer
```

The knowledge base contains AFM platform information such as:

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

The knowledge base is maintained separately from the APA application so documentation can evolve without coupling it to runtime application code.

---

# ☁️ APA Dynamic Infrastructure Access

When a question requires current state, APA uses controlled read-only integrations.

## Kubernetes

The Kubernetes integration can retrieve supported information such as:

- Nodes
- Pods
- Deployments
- Services
- Namespaces
- Events
- Workload status

```text
APA
 ↓
Read-only Kubernetes Client
 ↓
Kubernetes API
 ↓
Current EKS State
```

## AWS

The AWS integration uses the AWS SDK/Boto3 to retrieve supported information from services such as:

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

```text
APA
 ↓
AWS SDK / Boto3
 ↓
AWS APIs
 ↓
Current AWS State
```

The exact API surface is intentionally restricted to approved read-only operations.

---

# 🔐 APA Security Model

## Kubernetes authorization

```text
APA Pod
   ↓
Kubernetes RBAC
   ↓
Read-only permissions
   ↓
Kubernetes API
```

APA does not receive broad Kubernetes administrator permissions.

## AWS authorization

```text
APA Pod
   ↓
EKS Pod Identity
   ↓
IAM Role
   ↓
Least-Privilege Read Permissions
   ↓
AWS APIs
```

APA does not require long-lived AWS access keys inside the container.

The AWS IAM policy should expose only the read operations required by the supported APA tools.

---

# 🚀 APA Deployment and Delivery

APA is maintained as a separate application repository and follows the same GitLab CI/CD and GitOps principles as the rest of AFM.

```text
APA Repository
      ↓
GitLab CI/CD
      ↓
Build / Validation
      ↓
Docker Image
      ↓
Amazon ECR
      ↓
GitOps Repository
      ↓
Argo CD
      ↓
Amazon EKS
      ↓
APA Pod
```

APA runs on dedicated EKS worker capacity.

The OpenAI API credential is supplied securely at runtime and is not embedded into source code or the Docker image.

---

# 🧬 APA Evolution

APA was developed in two major stages.

## Stage 1 — Local Static APA

The initial implementation was a local RAG application using Ollama and Qwen.

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

This stage established the retrieval, vector-store and AI application foundation.

## Stage 2 — EKS-hosted Dynamic APA

The final AFM v3 implementation moved LLM inference to the OpenAI API and introduced controlled live infrastructure access.

```text
Streamlit
    ↓
Query Router
    ↓
 ┌───────────────┬──────────────────┐
 │               │                  │
Static RAG     Kubernetes          AWS
 │               │                  │
ChromaDB       K8s API            Boto3
 │               │                  │
 └───────────────┴──────────────────┘
                 ↓
          Retrieved Evidence
                 ↓
            OpenAI API
                 ↓
            GPT-4o-mini
                 ↓
          Grounded Response
```

The current AFM v3 APA implementation is treated as the **read-only baseline/final phase of the platform**.

---

# 🧭 APA's Role in the AFM Operations Stack

APA does not replace the existing observability and deployment tools.

Instead, each tool has a distinct role:

| Tool | Primary role |
|---|---|
| Prometheus | Metrics collection and alert evaluation |
| Grafana | Visualization and dashboards |
| Alertmanager | Alert routing and notifications |
| Slack | Operational notification channel |
| Argo CD | GitOps reconciliation and deployment |
| Kubernetes | Workload orchestration |
| AWS | Infrastructure platform |
| APA | Natural-language retrieval and explanation of platform knowledge/current read-only state |

For example:

```text
Prometheus detects an alert
        ↓
Alertmanager routes it
        ↓
Slack notifies the engineer
        ↓
Engineer asks APA for context
        ↓
APA retrieves relevant runbook + current state
        ↓
APA explains the observed condition
```

APA therefore acts as an additional **engineering interface**, not as a replacement for the underlying observability or deployment systems.

---

# 🧪 Operational Challenges and Engineering Decisions

AFM v3 evolved through practical engineering problems rather than being designed only as a static architecture diagram.

## Kubernetes pod capacity

The selected cost-conscious EKS worker size created pod-capacity pressure.

The platform was improved through:

- VPC CNI prefix delegation
- Custom max-pods configuration
- Worker-node launch-template configuration
- Additional node capacity
- Workload separation for APA

## Prometheus Operator CRDs

Prometheus Operator CRDs created Argo CD synchronization challenges.

The observability deployment was adjusted to control CRD installation and use compatible Argo CD synchronization behavior.

## Infrastructure security

Security scanning was expanded beyond application/container scanning to Terraform configuration through Trivy IaC.

## Secrets and identity

The platform evolved toward AWS-native secret management and workload identity:

- AWS Secrets Manager
- External Secrets
- EKS Pod Identity
- IAM
- Kubernetes RBAC

## HTTPS and domain

The platform evolved from a basic application endpoint into a custom-domain architecture using:

- Route 53
- ACM
- HTTPS
- ALB

## GitOps

Application delivery evolved into a pull-based GitOps model where Git remains the source of truth for Kubernetes desired state.

## AI operations

APA evolved from a local static RAG implementation into an EKS-hosted OpenAI-powered assistant with controlled live AWS and Kubernetes read-only integrations.

---

# 📸 Portfolio Evidence

The repository should contain visual evidence supporting the claims made in this README.

## Architecture evidence

`diagrams/`

Recommended evidence includes:

- Overall AWS architecture
- Application architecture
- Terraform lifecycle
- CI/CD pipeline
- GitOps flow
- Observability architecture
- Security architecture
- APA architecture
- APA static/dynamic workflow

## Pipeline evidence

`pipeline-docs/`

Recommended evidence includes:

- Infrastructure pipeline
- Application pipeline
- Observability pipeline
- APA pipeline

## Screenshot evidence

`screenshots/afm-project/`

Recommended screenshots include:

- AWS infrastructure
- EKS
- GitLab CI/CD
- Argo CD
- Prometheus
- Grafana
- Alertmanager
- Slack alerts
- SonarQube
- Trivy
- OWASP ZAP
- APA UI
- APA dynamic Kubernetes/AWS responses

## Project documentation

`Project-documents/`

Contains the detailed project documentation and supporting material used to build the platform knowledge base.

---

# 🧮 Portfolio Metrics

Rather than hard-coding potentially changing repository/resource counts into this README, the portfolio focuses on the architecture and capabilities that can be verified from the repositories and screenshots.

The current platform includes:

- 4 AFM application components
- A separate APA application
- A separate APA knowledge base
- Multiple Terraform modules
- Multiple GitLab CI/CD pipelines
- Amazon ECR image repositories
- Amazon EKS workloads
- Prometheus/Grafana/Alertmanager/YACE observability
- Multiple DevSecOps controls
- AWS-managed services across networking, compute, database, identity, storage and monitoring

---

# 🧭 Architecture Principles

AFM v3 follows these engineering principles:

### Infrastructure as Code

AWS infrastructure is managed through Terraform.

### GitOps

Kubernetes desired state is maintained in Git and reconciled by Argo CD.

### Least Privilege

AWS IAM and Kubernetes RBAC are used to restrict access.

### Secure Secrets

Sensitive credentials are managed through AWS Secrets Manager rather than being hard-coded into source code or images.

### Workload Identity

EKS Pod Identity provides AWS credentials to supported workloads without embedding long-lived access keys.

### Shift-Left Security

Security validation is integrated into application and infrastructure pipelines.

### Immutable Artifacts

Application images are built and stored in Amazon ECR and referenced by versioned image tags.

### Centralized Observability

Prometheus and Grafana provide centralized metrics and visualization, with Alertmanager/Slack handling notifications.

### Cost Awareness

The platform uses cost-conscious AWS resources and can destroy ephemeral infrastructure when it is not required.

### Read-Only AI Operations

APA is intentionally designed without infrastructure mutation or remediation capabilities.

---

# ⚠️ Known Limitations

AFM v3 is a production-inspired portfolio project and is **not a production banking deployment**.

Current limitations include:

- Refresh-token rotation and explicit token revocation/blacklisting are not implemented.
- Proxy services do not currently implement advanced circuit-breaker/fallback patterns such as Resilience4j.
- Blue-Green traffic switching is a controlled release process rather than a fully automated production progressive-delivery system.
- Internal service-to-service traffic currently uses HTTP.
- mTLS/service-mesh capabilities are not implemented.
- Distributed tracing is not currently implemented.
- Infrastructure is intentionally cost-conscious and not designed as a multi-AZ production banking environment.
- APA is strictly read-only and does not perform autonomous remediation.

---

# 🛣️ Future Roadmap

Potential future enhancements include:

- OAuth2 / OIDC integration
- Refresh-token rotation and revocation
- OpenTelemetry distributed tracing
- Automated Blue-Green traffic evaluation
- Progressive delivery based on Prometheus metrics
- Service mesh and mTLS
- Additional resilience patterns
- Expanded SRE automation
- Additional read-only APA tools
- Further AI-assisted operational workflows while preserving the read-only safety boundary

---

# 🎓 Skills Demonstrated

## Cloud / Platform Engineering

`AWS` · `VPC` · `EKS` · `EC2` · `RDS` · `ECR` · `S3` · `ALB` · `ACM` · `Route 53` · `Secrets Manager` · `IAM`

## Infrastructure as Code

`Terraform` · `Terraform Modules` · `Remote State` · `S3 Native Locking` · `CI-driven Infrastructure`

## DevOps / GitOps

`GitLab CI/CD` · `Docker` · `Amazon ECR` · `Kubernetes` · `Argo CD` · `GitOps`

## DevSecOps

`SonarQube` · `Trivy` · `Trivy IaC` · `OWASP ZAP` · `IAM` · `Kubernetes RBAC` · `EKS Pod Identity` · `AWS Secrets Manager`

## Observability

`Prometheus` · `Grafana` · `Alertmanager` · `YACE` · `CloudWatch` · `Slack` · `Micrometer`

## Application Engineering

`Java 17` · `Spring Boot 3.3.11` · `Spring Security` · `JPA/Hibernate` · `PostgreSQL` · `JWT` · `BCrypt` · `Maven`

## AI / Platform Operations

`Python` · `Streamlit` · `RAG` · `Embeddings` · `ChromaDB` · `OpenAI API` · `GPT-4o-mini` · `Kubernetes API` · `AWS SDK/Boto3` · `Ollama` · `Qwen`

---

# 🏁 Final Takeaway

AFM v3 demonstrates the evolution of a cloud-native application platform into a GitOps-operated DevSecOps environment with integrated security, observability and AI-assisted platform operations.

The platform combines:

```text
Terraform
   +
AWS
   +
Amazon EKS
   +
GitLab CI/CD
   +
DevSecOps
   +
Argo CD / GitOps
   +
Prometheus / Grafana
   +
AWS Managed Services
   +
Microservices
   +
AI / RAG
   +
Read-Only Platform Operations
```

The **AFM Platform Assistant (APA)** adds a natural-language interface over AFM platform knowledge and current read-only AWS and Kubernetes information.

APA complements existing operational systems such as Prometheus, Grafana, Alertmanager, Argo CD and Kubernetes. It does not replace them and does not perform infrastructure-changing operations.

The result is a single portfolio project demonstrating:

- Cloud infrastructure engineering
- Infrastructure as Code
- Kubernetes/EKS
- CI/CD
- DevSecOps
- GitOps
- Observability
- Security and workload identity
- Microservice deployment
- AI/RAG
- Read-only AI-assisted platform operations

---

# 👨‍💻 Portfolio

**Swapnil Gavhale**

DevOps / Cloud / Platform Engineering

`AWS` · `Terraform` · `Kubernetes` · `EKS` · `GitLab CI/CD` · `Docker` · `Argo CD` · `GitOps` · `DevSecOps` · `Prometheus` · `Grafana` · `RAG` · `LLM` · `AI Platform Operations`

---

## Documentation Metadata

- **Documentation:** AFM v3 Portfolio Root README
- **Platform Version:** `3.0.0`
- **Current phase:** AFM v3 + read-only APA
- **Last reviewed:** August 2026
- **Maintainer:** Swapnil Gavhale
