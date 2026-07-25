# 🚀 AFM Application DevSecOps Pipeline

### CI/CD for AFM Microservices on Amazon EKS

---

## 🔰 Overview
The **AFM Application Pipeline** is responsible for building, securing, packaging, deploying, and validating the **AFM microservices** running on Amazon EKS.

Unlike simplistic CI/CD demos, this pipeline is designed to reflect **real DevSecOps workflows**, where:
- Security is enforced at multiple stages
- Builds are reproducible
- Deployments are controlled
- Runtime behavior is validated post-deployment

The pipeline applies to all AFM services:
- `frontend-ui`
- `login-service`
- `registration-service`
- `auth-service`

Each service follows the **same standardized pipeline**, ensuring consistency and scalability.

---

## 🎯 Design Principles

The AFM application pipeline was built with the following principles:
- **Shift-left security**
- **Immutable container images**
- **Separation of build, security, and runtime testing**
- **No manual server access**
- **Environment parity between services**
- **Cost-aware execution**

This mirrors how **enterprise DevOps teams** operate application platforms.

---
## 🧱 Pipeline Responsibilities

This pipeline is responsible for:
- Compiling AFM services
- Performing static and image security scans
- Building Docker images
- Publishing images to Amazon ECR
- Deploying services to EKS
- Running runtime security tests
- Producing security and scan reports

Infrastructure provisioning and observability setup are handled by **separate pipelines**.

---

## 🚦 High-Level Pipeline Flow

```
Code Commit
   ↓
Build & Test
   ↓
SAST (SonarQube)
   ↓
Docker Image Build
   ↓
Image Scan (Trivy)
   ↓
Push to Amazon ECR
   ↓
Deploy to Amazon EKS
   ↓
DAST (OWASP ZAP)
```


Each stage acts as a **quality and security gate**.

---

## 🛠️ Pipeline Stages (Detailed)

---

### 1️⃣ Source Code Commit
- Developers commit changes to the AFM repository
- Each microservice is maintained as a separate logical unit
- The pipeline is triggered manually or via controlled execution

This avoids uncontrolled deployments and reflects **production discipline**.

---
### 2️⃣ Build & Compile (Maven)

- Java-based AFM services are compiled using Maven
- Ensures:
    - Dependency resolution
    - Build reproducibility
    - Early failure detection

Build artifacts are **not deployed directly** — containers are used instead.

---

### 3️⃣ Static Application Security Testing (SAST)

- **SonarQube** scans source code
- Detects:
    - Code smells
    - Security hotspots
    - Vulnerable patterns

This stage enforces **code-level security before containerization**.

---

### 4️⃣ Docker Image Build
- Each AFM service is containerized
- Images are:
    - Immutable
    - Versioned
    - Tagged per pipeline execution

This ensures **build once, deploy everywhere** semantics.

---

### 5️⃣ Software Composition & Image Security Scan (SCA)

- **Trivy** scans Docker images
- Detects:
    - OS-level vulnerabilities
    - Library CVEs
    - Known insecure packages

Only scanned images are allowed to proceed.

---

### 6️⃣ Push to Amazon ECR
- Approved images are pushed to **Amazon ECR**
- ECR serves as the **single source of truth** for deployments
- Kubernetes pulls images directly from ECR

No images are built on cluster nodes.

---
### 7️⃣ Deployment to Amazon EKS

- Kubernetes manifests deploy AFM services into:
    - Namespace: `afm-bank`
- Services are deployed as:
    - Pods behind ClusterIP services
- Only the frontend is exposed via Ingress and ALB

This enforces **strong network boundaries**.

---

### 8️⃣ Runtime Security Testing (DAST)
- **OWASP ZAP** runs **after deployment**
- ZAP targets:
    - AWS Application Load Balancer URL
- Tests:
    - Authentication flows
    - Input validation
    - HTTP security headers
    - Runtime vulnerabilities

ZAP is **not part of the live traffic flow** and runs on-demand.

---

### 9️⃣ Security Report Generation
- SonarQube, Trivy, and ZAP generate:
    - HTML reports
    - JSON artifacts
- Reports are stored as pipeline artifacts
- Enables:
    - Auditing
    - Review
    - Learning from findings

---

## 🔐 Secrets & Configuration Management
- Secrets are managed using **GitLab CI/CD protected variables**
- No credentials are hardcoded in:
    - Source code
    - Docker images
    - Kubernetes manifests

Future enhancements include:
- AWS Secrets Manager
- IRSA-based access

---
## ⚠️ Real Challenges Encountered

### Pipeline Execution Constraints
- Single EC2 runner handling:
    - Builds
    - Docker images
    - Security scans
### Resolution
- Disk expansion (8 GB → 20 GB)
- Cleanup of unused Docker layers
- Controlled pipeline execution

These challenges mirror **real operational learning**.

---

## 🔁 Relationship with Other Pipelines

|Pipeline|Responsibility|
|---|---|
|**afm-infra-pipeline**|Provisions AWS & EKS|
|**afm-project-pipeline**|Builds & deploys AFM services|
|**afm-observability-pipeline**|Monitoring & alerting|

This separation ensures:
- Reduced blast radius
- Clear ownership
- Easier debugging

---

## 🧠 Why This Pipeline Matters

This pipeline demonstrates:
- Real DevSecOps layering
- Secure CI/CD design
- Kubernetes deployment maturity
- Runtime security validation
- Production-style discipline

It is **not a toy pipeline**, but a **controlled application delivery system**.

---

## 🔚 Final Takeaway

> The AFM application pipeline treats application delivery as a **secure, repeatable, and auditable process**, aligning with real-world DevSecOps practices.

### Single-Node Resource Constraint (Real Limitation Observed)
The EKS cluster was backed by a single **t3.medium** worker node (2 vCPU, 4 GB RAM).
Under this configuration, the cluster could reliably schedule **approximately 15–17 pods**, including:

- Kubernetes system pods
- AFM application pods
- Observability components (Prometheus, Grafana, CloudWatch Agent)

When the observability stack was initially deployed with default settings, the Kubernetes scheduler correctly prevented additional pods from being scheduled due to insufficient CPU and memory.

#### Resolution
- Reduced AFM replica counts
- Tuned resource requests and limits
- Optimized Prometheus and Grafana footprint

This demonstrated real-world capacity planning and scheduler behavior under constrained environments.
