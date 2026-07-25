# 🚀 AFM DevSecOps Project
### **Portfolio by Swapnil Gavhale**
-----------------------------------------------------------------------
## 🔰 Overview
The **AFM (App Feature / Microservice) Project** is a **constraint-driven DevSecOps portfolio**, designed to demonstrate **how real DevOps platforms are designed, evolved, secured, and operated** — not just how individual tools are used.
Unlike typical demo projects, AFM intentionally focuses on:
- **Architecture & design decisions**
- **Trade-offs under constraints**
- **Failures, bottlenecks, and recovery**
- **Cost-aware engineering**
- **Operational realism**

The platform was built **incrementally**, validated at each stage, and later migrated to Kubernetes using **production-grade DevOps practices**.

---------------------------------------------------------------------
## 1️⃣ Problem Statement & Motivation
Most publicly available DevOps projects focus on **tool demonstrations**, such as:
- Running containers
- Simple CI pipelines
- One-click Kubernetes setups

However, **real DevOps roles** require engineers to:
- Design systems under constraints
- Make architectural decisions
- Handle failures and incidents
- Balance **cost, security, and scalability**    
- Operate platforms with **limited resources**

### Motivation Behind AFM
The AFM project was created to:
- Simulate **enterprise-grade DevOps challenges**
- Build systems **incrementally**, not ideally
- Follow a realistic journey:  
    **Start small → evolve → migrate → optimize**
- Document **why decisions were taken**, not just _what was implemented_

The project intentionally embraces real-world constraints:
- Limited budget
- No DNS / domain
- Single-node Kubernetes cluster
- Real pipeline failures
- Hybrid manual + automated workflows

------------------------------------------------------------------

## 2️⃣ Architecture Choice – Why Microservices?

Microservices were chosen to expose **real CI/CD, security, and Kubernetes complexity**, not theoretical scalability.
### Why Microservices Instead of a Monolith?
Microservices enable:
- Independent build & deploy
- Service isolation
- Kubernetes-native workflows
- Real ingress & routing scenarios
- Non-trivial CI/CD orchestration

This closely mirrors how **Banking and enterprise platforms** are actually built and operated.
-----------------------------------------------------------------
## 3️⃣ AFM Scope – Why Only 4 Microservices?
The project deliberately limits scope to **four AFMs**, prioritizing **depth over breadth**.

### Selected AFMs
1. **Auth Service**
2. **Registration Service**
3. **Login Service**
4. **Frontend UI (separate microservice)**

### Why This Split?
- **Auth / Login / Registration**
    - Represent real identity and access workflows in banking systems
- **Frontend UI as a separate microservice**
    - Independent UI releases
    - Backend changes without UI rebuild
    - Real ingress routing use cases

--------------------------------------------------------------
## 4️⃣ Technology Choices – Why These Tools?
### Backend – Spring Boot (Java)
Chosen because:
- Widely used in enterprise & banking systems
- Mature ecosystem
- Strong tooling support
- Easy observability & security integration

### Frontend – HTML / CSS
Chosen intentionally to:
- Keep focus on **DevOps**, not frontend frameworks
- Enable fast iteration
- Simplify containerization
- Clearly demonstrate ingress routing

----------------------------------------------------------------
## 5️⃣ AWS as the Cloud Platform

AWS was selected to reflect **real enterprise adoption patterns**.
### AWS Services Used & Purpose
- **EC2** – GitLab CI/CD runner & initial hosting
- **S3** – Terraform remote state
- **DynamoDB** – Terraform state locking
- **ECR** – Container image registry
- **IAM** – Fine-grained access control
- **CloudWatch** – Infrastructure & cluster observability
- **RDS** – Persistent production-style database
- **EKS** – Kubernetes orchestration

---------------------------------------------------------------

## 6️⃣ Infrastructure as Code – Terraform Design
Infrastructure is managed **entirely via Terraform**, following enterprise IaC standards.
### Key Terraform Practice
- Modular design
- Environment-aware structure
- Remote state via S3
- DynamoDB state locking
- No local `terraform apply`
- All infrastructure changes via CI/CD only

This ensures:
- Reproducibility
- Auditability
- Safe team workflows

-------------------------------------------------------------
## 7️⃣ Infrastructure Automation – GitLab CI/CD
All infrastructure provisioning is executed **exclusively via GitLab pipelines**.
### Terraform Lifecycle in CI/CD
- **Validate** – Syntax & configuration checks
- **Plan** – Change preview
- **Apply** – Manual approval required
- **Post-Provision** – ALB Controller installation
- **Destroy** – Isolated, manual teardown

This mirrors **real platform-engineering workflows**.

-------------------------------------------------------------
## 8️⃣ EKS Cluster Design – Intentional Constraints
### Why a Single-Node EKS Cluster?
- Instance type: **t3.medium**
- Managed node group
- Cost-efficient
- Forces capacity planning
- Exposes real scheduling challenges

> Single-node EKS is **harder**, not easier — and that was intentional.

------------------------------------------------------------
## 9️⃣ Networking & Region Decisions
### Why Default VPC?
- Reduced networking complexity
- Avoided over-engineering
- Focus stayed on Kubernetes, CI/CD, and IaC

### Why `us-east-1`?
- Broad AWS service availability
- Cost-effective
- Faster access to new AWS features
- Enterprise-standard region

------------------------------------------------------------

## 🔟 Database Evolution – user.json → Amazon RDS
### Initial State
- Local `/data/user.json`
- Simple and fast for early validation

### Final State
- Amazon RDS (`db.t3.micro`)
- Shared backend for all AFMs
- Persistent, production-style storage
- Monitored via CloudWatch

-----------------------------------------------------------

## 1️⃣1️⃣ Container Registry – Why Amazon ECR?
Amazon ECR was chosen over GitLab / GitHub registries due to:
- Native AWS integration
- IAM-based authentication
- Seamless EKS image pulls
- Built-in vulnerability scanning
- Production suitability
-------------------------------------------------------------

## 1️⃣2️⃣ HTTPS & Ingress Journey
### Docker Compose Phase
- NGINX
- Self-signed certificates
- HTTPS enabled locally

### EKS Phase
- Migrated to **AWS Load Balancer Controller**
- Self-signed certs not supported by ALB
- ACM not used due to lack of DNS
- Operated over HTTP

### Why ALB Controller?
- Legacy EKS ingress controller deprecated (March 2026)
- AWS-recommended, future-proof solution
- Native AWS integration
- IRSA support
- Managed load balancers
-----------------------------------------------------------

## 1️⃣3️⃣ GitLab CI/CD Runner Design
- Single EC2 self-hosted runner
- Full Docker daemon control
- Terraform + Docker + EKS deployments
### Disk Expansion Learning
- Initial disk: 8 GB
- Expanded to 20 GB due to Docker layers, plugins, artifacts

Demonstrates **real operational learning**.
--------------------------------------------------------------

## 1️⃣4️⃣ Secrets Management Strategy
### Current State
- Secrets stored in **GitLab CI/CD variables**
- Masked & protected
- No hard-coded credentials

### Future Plan
- AWS Secrets Manager
- AWS KMS
- IRSA-based access

---------------------------------------------------------------
## 1️⃣5️⃣ Observability Strategy
### Tools Used

- **CloudWatch** – Infra & control plane
- **Prometheus** – Kubernetes & service metrics
- **Grafana** – Visualization & debugging

### Real Challenge Faced
- Single node could not schedule 17 pods
- Solution:
    - Reduced AFM replicas
    - Optimized observability footprint
    - Restored stability

-------------------------------------------------------------
## 🔚 Final Takeaway
> The AFM platform was built **under real constraints**, evolved through failures, and refined using automation, security, and observability — exactly how production DevOps systems are built.

This portfolio demonstrates:
- DevOps mindset
- Terraform & Kubernetes maturity
- CI/CD discipline
- DevSecOps integration
- Cost-aware engineering
- Future-ready platform thinking

*** Please refer screenshots for more clarity.
------------------------------------------------------------------------

### Why DNS, HTTPS, and AWS Secrets Manager Were Not Used (Intentional)

In the AFM project, **DNS and HTTPS were intentionally not implemented** due to practical constraints:

- No custom domain was purchased.
- AWS ACM requires a verified domain for certificate issuance.
- ALB does not support self-signed certificates in production mode.

As a result:
- HTTPS termination on ALB was not possible
- The platform was operated over HTTP for learning and validation purposes

Because of this:
- **AWS Secrets Manager was not strictly required**
- Secrets were securely managed using **GitLab CI/CD protected and masked variables**
- No secrets were hardcoded in source code or container images

This approach reflects a **cost-aware, constraint-driven design**, while keeping a clear migration path for:
- DNS onboarding
- HTTPS via ACM
- IRSA-based access to AWS Secrets Manager.
