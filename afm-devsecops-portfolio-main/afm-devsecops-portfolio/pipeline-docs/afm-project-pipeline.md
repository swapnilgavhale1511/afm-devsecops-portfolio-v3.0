# 🚀 AFM v3 Application DevSecOps GitOps Pipeline

### GitLab CI/CD · DevSecOps · Amazon ECR · Argo CD · Amazon EKS · APA

---

## 🔀 AFM v3 Application Pipeline Model

AFM v3 uses a standardized CI + GitOps delivery pipeline across all
application services, with service-specific capabilities where required.

```
                    AFM v3 APPLICATION PIPELINES
                              │
          ┌───────────────────┴───────────────────┐
          │                                       │
          ▼                                       ▼
   COMMON CI + GITOPS                        SERVICE-SPECIFIC
          │                                  CAPABILITIES
          │                                       │
 ┌────────┼────────┐                    ┌─────────┴─────────┐
 │        │        │                    │                   │
 ▼        ▼        ▼                    ▼                   ▼
AUTH    LOGIN   REGISTRATION        AUTH SERVICE       FRONTEND UI
 │        │        │                Blue-Green          OWASP ZAP
 │        │        │                + Rollback          + Quality Gate
 └────────┼────────┘
          │
          ▼
       GitLab CI
          │
          ▼
       SonarQube
          │
          ▼
      Docker Build
          │
          ▼
        Trivy
          │
          ▼
         ECR
          │
          ▼
     GitOps Update
          │
          ▼
       Argo CD
          │
          ▼
        EKS
```
---

## 🔰 Overview

The **AFM v3 Application DevSecOps Pipeline** is responsible for building, securing, packaging, and promoting the AFM application services through the **GitOps delivery workflow** running on Amazon EKS.

Unlike the earlier AFM v2 model, AFM v3 does not use the application CI pipeline to directly deploy Kubernetes workloads.

AFM v3 follows a **CI + GitOps separation model**:

```text
Application Repository
        ↓
GitLab CI/CD
        ↓
Build / Test / Security
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

The application pipeline is therefore responsible for producing a **validated, immutable application artifact** and updating the desired GitOps state.

**Argo CD is responsible for deployment and reconciliation.**

This separation provides:

* Independent application repositories
* Independent service pipelines
* Immutable container images
* Automated security validation
* GitOps-based deployment
* Declarative Kubernetes desired state
* Automatic Argo CD reconciliation
* Reduced deployment blast radius
* Clear separation between CI and CD

---

# 🏗️ AFM v3 Application Repository Model

A major architectural change in AFM v3 is that the application services are maintained in **separate repositories** rather than being managed as a single application repository.

The AFM application repositories include:

```text
afm-auth-service
afm-login-service
afm-registration-service
afm-frontend-ui
```

Each repository contains the source code and its own GitLab CI/CD pipeline.

Therefore:

```text
Developer
   ↓
Service Repository
   ↓
Service-specific GitLab Pipeline
   ↓
ECR Image
   ↓
GitOps Repository
   ↓
Argo CD
   ↓
EKS
```

A change to one service does not require rebuilding unrelated services.

For example:

```text
afm-auth-service change
        ↓
auth-service pipeline
        ↓
auth image
        ↓
GitOps update
        ↓
Argo CD
        ↓
auth-service deployment
```

The same model applies independently to login, registration, and frontend UI.

---

# 🎯 Design Principles

The AFM v3 application delivery architecture was designed around the following principles:

* **GitOps-first deployment**
* **CI/CD separation**
* **One repository per application service**
* **One pipeline per service**
* **Shift-left security**
* **Immutable container images**
* **Build once, deploy through GitOps**
* **Automated GitOps promotion**
* **No direct `kubectl apply` deployment from CI**
* **No manual EKS deployment**
* **Pre-deployment security validation with service-specific post-deployment validation where required**
* **Service-level isolation**
* **Controlled rollback capability**
* **Cost-aware execution**

This reflects a modern DevSecOps and platform engineering workflow rather than a traditional CI pipeline that directly modifies the Kubernetes cluster.

---

# 🧱 Pipeline Responsibilities

Each AFM service pipeline is responsible for:

* Checking out application source code
* Building the application
* Running automated validation
* Performing static code analysis
* Building the Docker image
* Scanning the Docker image for vulnerabilities
* Authenticating with Amazon ECR
* Pushing the validated image to ECR
* Updating the GitOps repository with the new image reference
* Allowing Argo CD to reconcile the change

The application pipeline **does not directly deploy Kubernetes resources**.

Infrastructure provisioning and observability deployment are handled separately.

---
🔀 Service Pipeline Profiles

AFM v3 uses a standardized CI/CD foundation across all application repositories, with service-specific stages where required.
| Service                    | Standard CI/CD Flow                                                      | Additional Capability                        |
| -------------------------- | ------------------------------------------------------------------------ | -------------------------------------------- |
| `afm-auth-service`         | Build → SonarQube → Docker → Trivy → ECR → GitOps Update → Argo CD → EKS | **Blue-Green + Rollback**                    |
| `afm-login-service`        | Build → SonarQube → Docker → Trivy → ECR → GitOps Update → Argo CD → EKS | Standard GitOps deployment                   |
| `afm-registration-service` | Build → SonarQube → Docker → Trivy → ECR → GitOps Update → Argo CD → EKS | Standard GitOps deployment                   |
| `afm-frontend-ui`          | Build → SonarQube → Docker → Trivy → ECR → GitOps Update → Argo CD → EKS | **Post-deployment OWASP ZAP + Quality Gate** |

SonarQube and Trivy are pre-deployment security controls, while OWASP ZAP is a frontend-specific post-deployment runtime security control.
---
# 🔄 AFM v3 CI/CD + GitOps Flow

The complete application delivery flow is:

```text
Developer Commit
       ↓
Service Git Repository
       ↓
GitLab CI/CD
       ↓
Pre-cleanup
       ↓
Maven Build / Application Build
       ↓
SonarQube
       ↓
Docker Build
       ↓
Trivy Image Scan
       ↓
Push Image to Amazon ECR
       ↓
GitOps Update
       ↓
GitOps Repository
       ↓
Argo CD
       ↓
Amazon EKS
       ↓
Running AFM Workload
```

For the frontend service, the pipeline additionally performs a post-deployment security scan:

```text
GitOps Update
       ↓
Argo CD
       ↓
Amazon EKS
       ↓
Application Available
       ↓
OWASP ZAP
       ↓
ZAP Report / Quality Gate
```

---

# 🚦 Pipeline Stages

The standard AFM v3 service pipeline follows the CI stages below.

---

## 1️⃣ Pre-Cleanup

The pipeline begins with a cleanup step to maintain a predictable CI runner environment.

The cleanup stage helps remove unnecessary artifacts and temporary Docker resources from previous executions.

This is particularly important because the AFM pipelines use a shared EC2-based GitLab Runner and perform:

* Maven builds
* Docker builds
* Trivy scans
* Security tooling

The objective is to reduce unnecessary disk consumption between pipeline executions.

---

# 2️⃣ Application Build

### Backend Services

The Java/Spring Boot services are built using **Maven**.

This stage validates:

* Source compilation
* Dependency resolution
* Application build integrity
* Build reproducibility

The backend services include:

* `afm-auth-service`
* `afm-login-service`
* `afm-registration-service`

Build artifacts are not deployed directly to EKS.

The application is packaged into a Docker image for deployment.

---

### Frontend UI

The `afm-frontend-ui` repository uses its frontend build process to validate and package the React-based application before containerization.

The resulting application is subsequently packaged into its Docker image.

---

# 3️⃣ Static Code Analysis — SonarQube

The application source is analyzed using **SonarQube**.

The purpose is to identify code-quality and security issues before the application is packaged and promoted.

SonarQube provides visibility into areas such as:

* Code quality
* Code smells
* Security hotspots
* Vulnerable coding patterns
* Maintainability issues

This represents the **shift-left security and quality** stage of the pipeline.

---

# 4️⃣ Docker Image Build

After the application build and code analysis succeed, the application is containerized.

Each service produces its own Docker image.

The image is built from the service repository and represents the immutable application artifact that will ultimately run on EKS.

The deployment model follows:

> **Build once → scan once → publish once → deploy through GitOps**

The Kubernetes platform does not build application images.

---

# 5️⃣ Trivy Container Image Scan

The generated Docker image is scanned using **Trivy** before it is promoted to ECR.

The scan checks the image for known vulnerabilities, including:

* Operating system vulnerabilities
* Package vulnerabilities
* Dependency vulnerabilities
* Known CVEs

The security scan is performed before the image becomes the deployment artifact referenced by GitOps.

This provides a security gate between:

```text
Docker Build
     ↓
Trivy Scan
     ↓
Amazon ECR
```

An image should not be promoted as the deployment artifact when the configured security gate fails.

---

# 6️⃣ Push Image to Amazon ECR

After successful build and security validation, the image is pushed to **Amazon Elastic Container Registry (ECR)**.

ECR acts as the centralized registry for AFM application container images.

The flow is:

```text
Service Repository
       ↓
GitLab CI
       ↓
Docker Image
       ↓
Trivy
       ↓
Amazon ECR
```

EKS workloads pull the approved image from ECR during deployment.

No application image is built directly on an EKS worker node.

---

# 🏷️ Container Image Tagging

AFM v3 uses **immutable pipeline-based image identification**.

Images are tagged using the Git commit SHA rather than relying on a mutable latest tag.

Conceptually:

```text
afm-auth-service:<commit-sha>
afm-login-service:<commit-sha>
afm-registration-service:<commit-sha>
afm-frontend-ui:<commit-sha>
```

This provides:

* Traceability
* Reproducibility
* Clear source-to-image mapping
* Safer GitOps deployments
* Easier rollback

A GitOps deployment can therefore identify exactly which application revision is running.

---

# 7️⃣ GitOps Update

For the standard AFM service pipeline, GitOps Update is the final deployment-related CI stage.

After the GitOps repository is updated, Argo CD becomes responsible for deploying and reconciling the application in EKS. The afm-frontend-ui pipeline has an additional post-deployment OWASP ZAP validation stage that runs after Argo CD has deployed the application.

Conceptually:

```text
Application Repository
        ↓
GitLab CI
        ↓
ECR Image
        ↓
GitOps Repository Update
```

The GitOps repository becomes the **desired-state source of truth for Kubernetes deployment**.

The application pipeline does not execute:

```text
kubectl apply
kubectl rollout restart
kubectl scale
```

as its normal deployment mechanism.

Instead, it changes Git.

---

# 🔄 Argo CD Deployment

After the GitOps repository is updated, **Argo CD detects the desired-state change**.

Argo CD then reconciles the Kubernetes environment.

```text
GitOps Repository
       ↓
     Argo CD
       ↓
Desired Kubernetes State
       ↓
Amazon EKS
       ↓
AFM Workload
```

This creates a clean separation:

| Responsibility            | System                 |
| ------------------------- | ---------------------- |
| Source code               | Service Git repository |
| Build                     | GitLab CI              |
| Code quality/security     | SonarQube              |
| Container security        | Trivy                  |
| Image registry            | Amazon ECR             |
| Kubernetes desired state  | GitOps repository      |
| Deployment/reconciliation | Argo CD                |
| Runtime platform          | Amazon EKS             |

---

# 🔁 GitOps Reconciliation

The deployment model is declarative.

The GitOps repository defines what should be running.

Argo CD continuously compares:

```text
Desired State
     ↕
Live Kubernetes State
```

If the cluster differs from the Git-defined desired state, Argo CD can identify the drift and reconcile it according to the configured GitOps policy.

This provides an important operational property:

> **Git is the source of truth for application deployment state.**

---

# 🔵🟢 Blue-Green Deployment — Auth Service

AFM v3 includes a dedicated **blue-green deployment capability for `afm-auth-service`**.

This is intentionally limited to the authentication service and is not presented as a universal deployment mechanism for every AFM service.

The blue-green approach provides two application environments:

```text
             Traffic
                │
                ▼
          Active Version
                │
        ┌───────┴───────┐
        │               │
       BLUE            GREEN
      Version          Version
```

A new auth-service version can be introduced alongside the currently active version.

The deployment process can then validate the new version before switching traffic.

This reduces deployment risk compared with replacing the currently running version immediately.

---

# ↩️ Auth Service Rollback

The `afm-auth-service` implementation also includes a dedicated **rollback script**.

The rollback capability is intentionally scoped to auth-service.

Its purpose is to provide a controlled recovery mechanism when a newly promoted auth-service version causes unexpected behavior.

Conceptually:

```text
Current Version
      ↓
New Version
      ↓
Validation
      ↓
Failure
      ↓
Rollback
      ↓
Previous Known-Good Version
```

This demonstrates an important DevOps principle:

> **Deployment is incomplete without a recovery strategy.**

The rollback mechanism is not claimed as a generic rollback implementation for every AFM service.

---

# 🛡️ OWASP ZAP — Frontend UI Only

AFM v3 integrates **OWASP ZAP** as a post-deployment Dynamic Application Security Testing stage for the **`afm-frontend-ui` service only**.

ZAP is intentionally not included as a universal stage in every service pipeline.

The frontend pipeline follows:

```text
Build
  ↓
SonarQube
  ↓
Docker Build
  ↓
Trivy
  ↓
ECR
  ↓
GitOps Update
  ↓
Argo CD
  ↓
EKS Deployment
  ↓
OWASP ZAP
```

ZAP targets the deployed application through its externally accessible application endpoint.

The scan is therefore performed against the **running application**, rather than against source code or the Docker image.

---

# 🔎 OWASP ZAP Baseline Scan

The frontend pipeline uses an OWASP ZAP baseline scan.

The scan generates reports including:

* HTML report
* JSON report

The scan is configured as a post-deployment validation stage.

The purpose is to identify runtime web application security issues such as:

* Security header problems
* Common web vulnerabilities
* Input-related security findings
* Other issues detectable by the configured ZAP baseline scan

ZAP is not part of the normal application traffic path.

It is a **pipeline security validation tool**.

---

# 🚦 ZAP Quality Gate

AFM v3 also includes configurable ZAP quality-gate behavior.

The quality gate evaluates ZAP findings using configurable severity thresholds such as:

```text
ZAP_FAIL_ON_HIGH
ZAP_FAIL_ON_MEDIUM
ZAP_FAIL_ON_LOW
```

This allows the pipeline to distinguish between:

* Informational findings
* Findings that should be reviewed
* Findings that should fail the pipeline

For afm-frontend-ui, the ZAP scan functions as a post-deployment security quality gate rather than merely producing a report.

---

# 📦 Pipeline Artifacts and Reports

The security and validation stages generate reports that can be retained as GitLab CI artifacts.

Examples include:

* SonarQube analysis results
* Trivy scan results
* OWASP ZAP HTML report
* OWASP ZAP JSON report

These artifacts provide evidence of the security checks performed during the application delivery process.

They support:

* Troubleshooting
* Security review
* Pipeline auditing
* Portfolio demonstration
* Historical investigation

---

# 🔐 Secrets and Configuration Management

The AFM v3 application pipelines avoid embedding sensitive credentials directly into source code or container images.

CI/CD credentials and protected values are managed through appropriate GitLab CI/CD mechanisms.

Runtime application secrets are handled separately through the AWS/Kubernetes platform architecture.

For example, AFM application components that require AWS access use the platform's workload identity mechanisms rather than embedding long-lived AWS credentials into application images.

This maintains separation between:

```text
CI/CD Credentials
        ≠
Application Runtime Secrets
        ≠
AWS Workload Identity
```

---

# ☁️ Relationship with AWS Infrastructure

The application pipeline does not provision the underlying AWS infrastructure.

Persistent AWS resources such as:

* VPC/networking
* RDS PostgreSQL
* ECR
* S3
* AWS Secrets Manager
* Route 53
* ACM
* ALB-related infrastructure

are handled through the separate AFM infrastructure/platform lifecycle.

The application pipeline consumes the infrastructure that has already been provisioned.

This keeps application delivery decoupled from infrastructure provisioning.

---

# ☸️ Relationship with Amazon EKS

The application pipeline ultimately delivers workloads to the EKS platform, but it does so indirectly through GitOps.

The architecture is:

```text
GitLab CI
    │
    ├── Build
    ├── Test
    ├── SonarQube
    ├── Docker Build
    ├── Trivy
    ├── ECR Push
    │
    └── GitOps Update
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
              ▼
          AFM Services
```

This is the defining difference between the AFM v2 and AFM v3 deployment model.

---

# 🤖 Relationship with AFM Platform Assistant (APA)

The **AFM Platform Assistant (APA)** is a separate operational capability built around the AFM v3 platform.

APA is not responsible for building or deploying application images.

The application pipelines therefore remain independent of APA.

APA can consume approved **read-only operational information** from the deployed platform and supporting systems to help answer questions about:

* Kubernetes/EKS state
* Application workloads
* AWS infrastructure
* Argo CD state
* Observability information
* AFM platform configuration

The relationship is:

```text
Application CI/CD
       ↓
GitOps
       ↓
Argo CD
       ↓
EKS
       ↓
Running AFM Platform
       ↓
APA
       ↓
Read-Only Operational Information
```

APA does not modify the application deployment pipeline and does not perform deployment, rollback, sync, restart, scale, patch, or infrastructure mutation operations.

APA is documented separately in the dedicated **AFM Platform Assistant documentation**.

---

# 🔗 Relationship with Other AFM Pipelines

AFM v3 separates responsibilities across multiple pipelines.

| Pipeline / System               | Primary Responsibility                                                                                    |
| ------------------------------- | --------------------------------------------------------------------------------------------------------- |
| **AFM Infrastructure Pipeline** | AWS infrastructure and EKS platform lifecycle                                                             |
| **AFM Service Pipelines**       | Build, secure, package, publish, and update GitOps state for individual AFM services                      |
| **AFM Observability GitOps**    | Deploy and manage Prometheus, Grafana, Alertmanager, YACE and observability configuration through Argo CD |
| **Argo CD**                     | GitOps deployment and Kubernetes reconciliation                                                           |
| **APA**                         | Read-only operational and platform information                                                            |

This separation provides:

* Reduced blast radius
* Independent service delivery
* Clear ownership
* Easier troubleshooting
* Reproducible deployments
* Cleaner GitOps boundaries

---

# 🧩 AFM v3 Pipeline Per Service

The resulting service architecture is:

```text
┌───────────────────────────┐
│ afm-auth-service repo     │
│ GitLab CI pipeline        │
└─────────────┬─────────────┘
              │
              ▼
             ECR
              │
              ▼
        GitOps Update
              │
              ▼
           Argo CD
              │
              ▼
             EKS


┌───────────────────────────┐
│ afm-login-service repo    │
│ GitLab CI pipeline        │
└─────────────┬─────────────┘
              │
              ▼
             ECR
              │
              ▼
        GitOps Update
              │
              ▼
           Argo CD
              │
              ▼
             EKS


┌───────────────────────────┐
│ afm-registration repo     │
│ GitLab CI pipeline        │
└─────────────┬─────────────┘
              │
              ▼
             ECR
              │
              ▼
        GitOps Update
              │
              ▼
           Argo CD
              │
              ▼
             EKS


┌───────────────────────────┐
│ afm-frontend-ui repo      │
│ GitLab CI pipeline        │
└─────────────┬─────────────┘
              │
              ▼
             ECR
              │
              ▼
        GitOps Update
              │
              ▼
           Argo CD
              │
              ▼
             EKS
              │
              ▼
          OWASP ZAP
```

This structure ensures that each service can be developed, tested, secured, and promoted independently.

---

# 🧪 Pipeline Validation

The AFM v3 application delivery workflow validates the application at multiple points.

### Build Validation

Confirms that the application can be successfully built.

### Code Quality Validation

SonarQube analyzes the source code.

### Container Security Validation

Trivy scans the generated Docker image.

### Registry Validation

The validated image is published to ECR.

### GitOps Validation

The GitOps repository is updated with the intended image reference.

### Deployment Validation

Argo CD reconciles the desired state into EKS.

### Runtime Security Validation

The frontend pipeline performs OWASP ZAP validation after deployment.

### Recovery Validation

The auth-service deployment model provides blue-green deployment and rollback capability.

---

# ⚠️ Real Engineering Challenges

AFM v3 was designed and tested under practical development constraints.

The application delivery environment includes:

* A shared EC2-based GitLab Runner
* Docker image builds
* Maven builds
* SonarQube analysis
* Trivy scanning
* Multiple independent service pipelines
* A two-node EKS platform
* GitOps reconciliation
* Observability workloads

The two-node EKS architecture provides additional scheduling capacity compared with the original single-node implementation while still keeping the platform cost-conscious.

Resource usage therefore remains an important consideration when running the complete AFM platform.

The solution was to deliberately control:

* Application replica counts
* Observability resource consumption
* Prometheus retention
* Persistent storage
* Pipeline cleanup
* Supporting platform workloads

This demonstrates practical capacity and cost management rather than assuming unlimited cloud resources.

---

# 🧠 Why the AFM v3 Pipeline Matters

The AFM v3 application pipeline demonstrates several modern DevOps and platform engineering practices.

## CI/CD

GitLab CI provides automated application build and security workflows.

## DevSecOps

Security is integrated into the pipeline using:

* SonarQube
* Trivy
* OWASP ZAP for frontend runtime validation

## Containerization

Applications are packaged as immutable Docker images.

## Cloud Registry

Amazon ECR provides centralized image storage.

## GitOps

The application pipeline updates desired deployment state in Git instead of directly applying Kubernetes resources.

## Continuous Reconciliation

Argo CD continuously manages the relationship between Git-defined desired state and the EKS cluster.

## Progressive Delivery

Auth-service includes blue-green deployment capability.

## Recovery

Auth-service includes a dedicated rollback mechanism.

## SRE / Operations

The deployed applications are monitored through the AFM v3 observability platform.

## AI-Assisted Operations

APA provides a separate read-only operational interface over approved live platform information.

---

# 🏆 Key Engineering Outcomes

The AFM v3 application delivery architecture demonstrates:

* **Separate repository per AFM service**
* **Independent GitLab CI/CD pipeline per service**
* **Automated Maven/application builds**
* **SonarQube static analysis**
* **Docker image creation**
* **Trivy container vulnerability scanning**
* **Amazon ECR image publishing**
* **Immutable Git SHA-based image identification**
* **GitOps repository update**
* **Argo CD-based deployment**
* **No normal direct CI-to-EKS deployment**
* **Declarative Kubernetes desired state**
* **Blue-green deployment for auth-service**
* **Rollback capability for auth-service**
* **OWASP ZAP post-deployment scanning for frontend-ui**
* **Configurable ZAP quality gates**
* **Security and scan report artifacts**
* **Two-node EKS compatibility**
* **Cost-aware pipeline execution**
* **Separation of infrastructure, application, and observability responsibilities**
* **Read-only operational visibility through APA**

---

# 🔚 Final Takeaway

> **AFM v3 transforms application delivery from a traditional CI/CD deployment pipeline into a secure CI + GitOps delivery model.**

Each AFM service has its own repository and independent GitLab pipeline.

The service pipeline builds and validates the application, creates and scans an immutable Docker image, publishes it to Amazon ECR, and finally updates the GitOps repository.

From that point onward, **Argo CD becomes responsible for deployment and reconciliation into Amazon EKS**.

The resulting delivery chain is:

```text
Code
 ↓
GitLab CI
 ↓
Build & Test
 ↓
SonarQube
 ↓
Docker Build
 ↓
Trivy
 ↓
Amazon ECR
 ↓
GitOps Update
 ↓
Argo CD
 ↓
Amazon EKS
```

With service-specific security and deployment capabilities:

```text
auth-service
    └── Blue-Green + Rollback

frontend-ui
    └── OWASP ZAP + Quality Gate
```

And the deployed platform is subsequently supported by:

```text
Prometheus
Grafana
Alertmanager
YACE
Slack
        ↓
      APA
        ↓
Read-Only Operational Visibility
```

This architecture demonstrates a complete modern application delivery lifecycle combining **GitLab CI/CD, DevSecOps, containers, Amazon ECR, Kubernetes, GitOps, Argo CD, progressive delivery, rollback, runtime security testing, observability, and AI-assisted read-only operations**.

It is therefore representative of a practical **DevOps/SRE platform engineering workflow**, rather than a simple build-and-deploy CI/CD demonstration.
