🤖 AFM Platform Assistant (APA)
Read-Only AI Operations Assistant · OpenAI API · RAG · Kubernetes · AWS · Argo CD · Prometheus · Grafana
🔰 Overview

The AFM Platform Assistant (APA) is a read-only AI-powered operations assistant built on top of the AFM v3 platform.

APA is designed to provide engineers with a natural-language interface for understanding the current state of the AFM platform, Kubernetes/EKS environment, AWS infrastructure, GitOps deployment state, and observability data.

Instead of requiring an engineer to manually inspect multiple systems and execute numerous CLI commands, APA provides a unified conversational interface for retrieving approved operational information.

APA combines:

Static AFM knowledge
RAG-based documentation retrieval
Live Kubernetes/EKS information
Live AWS information
Argo CD state
Prometheus and observability information
OpenAI-powered reasoning and response generation

The most important design principle is:

APA is an information and operations-assistance system, not an autonomous remediation system.

APA is intentionally read-only.

It does not execute operational changes against the AFM platform or AWS infrastructure.

🎯 APA Design Goals

APA was designed around the following goals:

Provide a natural-language interface for AFM platform operations
Combine static documentation with current live platform information
Reduce the need to manually inspect multiple operational systems
Provide current Kubernetes/EKS information
Provide current AWS infrastructure information
Provide GitOps/Argo CD deployment visibility
Provide observability information
Support troubleshooting and operational investigation
Maintain a strict read-only security boundary
Use AWS-native workload identity rather than static AWS credentials
Run as a Kubernetes workload on EKS
Remain independently deployable from the AFM application services
Keep AI-assisted operations separate from application CI/CD responsibilities
🧠 What Problem APA Solves

Traditional platform troubleshooting often requires an engineer to switch between multiple tools:

kubectl
AWS CLI
Argo CD
Prometheus
Grafana
Git
Runbooks
Architecture Documentation

For example, investigating an application issue may require answering:

Is the application pod running?
Which node is hosting it?
Are pods restarting?
What is the current deployment state?
Is Argo CD synchronized?
Is the ALB healthy?
Is RDS experiencing resource pressure?
What does the platform documentation say about this service?
Is there an existing troubleshooting procedure?

APA brings these information sources together into a single conversational interface.

Conceptually:

Engineer
   ↓
Natural-Language Question
   ↓
APA
   ↓
┌───────────────────────────────────────────────┐
│ Static Knowledge + Live Operational Evidence  │
└───────────────────────────────────────────────┘
   ↓
Reasoned Operational Response

The objective is to reduce information retrieval and context-switching overhead, not to replace the engineer's authority over the platform.

🏗️ High-Level Architecture

APA is deployed as an actual Kubernetes workload inside the AFM v3 EKS environment.

The high-level architecture is:

                         Engineer
                            │
                            ▼
                    ┌───────────────┐
                    │      APA      │
                    │  AI Assistant │
                    └───────┬───────┘
                            │
              ┌─────────────┴─────────────┐
              │                           │
              ▼                           ▼
       Static Knowledge              Live Evidence
              │                           │
        RAG / ChromaDB            ┌────────┼─────────┐
                                  │        │         │
                                  ▼        ▼         ▼
                              Kubernetes AWS     Observability
                                  │        │         │
                                  ▼        ▼         ▼
                               EKS APIs  AWS APIs Prometheus/
                                                  Grafana
                                  │
                                  ▼
                             Argo CD State
              │
              └─────────────┬─────────────┘
                            ▼
                     OpenAI API
                            │
                            ▼
                  Operational Response

APA therefore combines two complementary knowledge paths:

Static Knowledge

Information that changes relatively infrequently:

AFM architecture
Service documentation
Repository information
Terraform documentation
GitOps documentation
Runbooks
Troubleshooting procedures
Platform design documentation
Dynamic Knowledge

Information that must be retrieved from the live environment:

Current pods
Current nodes
Workloads
Services
EKS state
AWS resource information
Argo CD application state
Prometheus information
Grafana/observability information

This distinction is fundamental to APA.

🧩 Static Knowledge / RAG Layer

APA retains the existing static AFM knowledge capability using a RAG architecture.

The knowledge layer is based on:

AFM documentation
Architecture information
Runbooks
Operational procedures
Project-specific technical knowledge
ChromaDB vector storage
Embedding/retrieval
OpenAI-powered response generation

The RAG layer is useful for questions where the answer exists in project documentation rather than in the current runtime state.

For example:

"How is the AFM infrastructure provisioned?"

or:

"What is the GitOps deployment flow?"

or:

"How should a CrashLoopBackOff incident be investigated?"

These questions can be answered using the static knowledge base.

⚡ Dynamic Live Information Layer

Static RAG alone cannot answer questions that depend on the current platform state.

For example:

"How many pods are currently running?"

"Which node is the auth-service pod running on?"

"Is the Argo CD application synchronized?"

"What is the current RDS CPU utilization?"

"Are there unhealthy ALB targets?"

For these questions, APA uses approved read-only live integrations.

The dynamic layer can retrieve current information from:

Kubernetes/EKS
AWS
Argo CD
Prometheus
Grafana/observability systems

The result is a hybrid model:

                 User Question
                      │
                      ▼
                     APA
                      │
            ┌─────────┴─────────┐
            │                   │
            ▼                   ▼
      Static Knowledge     Live Evidence
        / RAG Layer        / Dynamic APIs
            │                   │
            └─────────┬─────────┘
                      ▼
                OpenAI API
                      │
                      ▼
             Contextual Answer
🤖 OpenAI Integration

APA uses the OpenAI API as the language-model layer.

The model is responsible for:

Understanding the user's question
Interpreting retrieved information
Combining relevant static and live context
Producing a natural-language response
Explaining operational information clearly

The model itself does not receive unrestricted infrastructure access.

Instead, APA controls which information is retrieved and supplied to the model.

This creates an important separation:

OpenAI
   ↓
Reasoning / Response Generation


APA Integrations
   ↓
Controlled Read-Only Evidence

The AI model is therefore not treated as an unrestricted infrastructure operator.

🔐 Strict Read-Only Security Model

Read-only access is the core security boundary of APA.

APA must not perform operations that modify platform state.

APA must not:
Restart pods
Restart deployments
Scale workloads
Patch resources
Apply Kubernetes manifests
Delete resources
Create resources
Modify ConfigMaps
Modify Secrets
Modify Services
Modify Ingress resources
Modify Argo CD applications
Sync Argo CD applications
Roll back Argo CD applications
Modify Prometheus configuration
Modify Grafana configuration
Modify Alertmanager configuration
Modify AWS resources
Create AWS resources
Delete AWS resources
Modify IAM configuration
Modify networking
Modify RDS
Modify S3
Modify ECR
Execute infrastructure Terraform changes

APA is therefore fundamentally different from an autonomous remediation agent.

Its role is:

Observe → Understand → Explain

not:

Observe → Decide → Modify

☸️ APA as a Kubernetes Workload

APA runs as an actual Kubernetes pod in the AFM v3 EKS cluster.

The dedicated namespace is:

afm-platform-assistant

The architecture is:

Amazon EKS
     │
     ▼
┌────────────────────────────────┐
│ afm-platform-assistant         │
│                                │
│       APA Pod                  │
│          │                     │
│          ▼                     │
│   Kubernetes ServiceAccount    │
│          │                     │
│          ▼                     │
│    EKS Pod Identity            │
│          │                     │
│          ▼                     │
│ APA Read-Only IAM Role         │
└────────────────────────────────┘

APA therefore runs using Kubernetes-native workload identity rather than storing AWS access keys inside the application.

🔑 AWS Identity — EKS Pod Identity

APA uses Amazon EKS Pod Identity for AWS access.

It does not use IRSA.

The identity flow is:

APA Pod
   ↓
Kubernetes ServiceAccount
   ↓
EKS Pod Identity
   ↓
APA Read-Only IAM Role
   ↓
AWS APIs

This provides a clean separation between:

Application identity
Kubernetes identity
AWS identity

No long-lived AWS access key or secret key is embedded inside the APA container.

🛡️ APA Read-Only IAM Role

The IAM role associated with APA is designed specifically for read-only AWS operations.

The role should grant only the AWS API permissions required by APA's approved live integrations.

The principle is:

Least privilege + read-only access.

APA does not receive administrative AWS permissions simply because it is an AI assistant.

This significantly reduces the potential blast radius of:

Application bugs
Prompt manipulation
Incorrect reasoning
Model hallucination
Credential compromise

The AI layer therefore operates behind a deliberately restricted AWS identity boundary.

☸️ Kubernetes Read-Only Access

APA also requires Kubernetes visibility to answer questions about the running platform.

Kubernetes access is similarly designed around read-only permissions.

The APA workload can retrieve information required to understand:

Pods
Deployments
ReplicaSets
Services
Ingress
Nodes
Namespaces
Workload status
Events
Other approved platform resources

The intent is to allow APA to inspect the platform without modifying it.

The authorization model therefore follows:

APA
 ↓
Kubernetes ServiceAccount
 ↓
Read-Only RBAC
 ↓
Kubernetes API
☁️ AWS Live Integrations

APA can retrieve approved AWS information through its read-only AWS identity.

The AWS information layer is intended to provide current information about the AFM infrastructure.

Depending on the approved integration scope, APA can retrieve information about resources such as:

EKS
EC2
RDS
ALB
ECR
S3
Secrets Manager metadata
Route 53
ACM
Other approved AFM AWS resources

APA is not intended to retrieve or expose secret values. Where Secrets Manager is queried, access is limited to approved metadata required for operational visibility.
The objective is not unrestricted AWS discovery.

The integration should expose only the AWS information required for operational visibility.

☸️ Kubernetes / EKS Live Integration

The Kubernetes integration provides current runtime information.

Examples include:

Number of pods
Pod status
Pod locations
Node status
Deployment status
Replica availability
Namespace information
Service information
Ingress information
Kubernetes events
Resource utilization where available

For example, an engineer could ask:

"How many AFM pods are running on each node?"

APA can retrieve the current Kubernetes state and provide the answer without requiring the engineer to manually execute:

kubectl get pods -A -o wide
🔄 Argo CD Live Integration

Argo CD is the GitOps control plane for AFM v3 application deployment.

APA can use approved read-only Argo CD information to answer questions about:

Application synchronization
Application health
Deployment state
Git revision
Desired state
Current state
Application resources
Sync status
Health status

For example:

"Is the auth-service Argo CD application synchronized?"

APA can retrieve the current Argo CD state and explain it.

APA must not use Argo CD access to perform:
- Sync
- Rollback
- Delete
- Create applications
- Modify applications

Argo CD access is strictly informational/read-only.

📊 Prometheus Integration

Prometheus is the central metrics layer for AFM v3.

APA can consume approved Prometheus information to answer questions involving current metrics.

Examples include:

Service availability
Request rate
Error rate
CPU utilization
Memory utilization
Pod health
Latency
p95/p99 latency
Selected RDS metrics
Selected ALB metrics

The architecture is:

AFM Platform
     ↓
Prometheus
     ↓
Read-Only APA Integration
     ↓
APA
     ↓
Operational Answer

APA does not modify Prometheus configuration.

📈 Grafana and Observability Relationship

Grafana provides the primary human-facing visualization layer for AFM observability. APA can consume approved Prometheus and observability data to provide conversational operational context; APA does not replace Grafana.

APA can use approved observability information to provide context around:

Application health
SRE metrics
RDS behavior
ALB behavior
Error rates
Latency
Resource utilization

Grafana remains the primary human visualization layer.

APA provides a conversational information layer on top of approved operational data.

The two capabilities are complementary:

Grafana
   ↓
Visual Operational Analysis


APA
   ↓
Conversational Operational Analysis
🗄️ RDS Operational Visibility

AFM v3 uses YACE to expose selected RDS PostgreSQL CloudWatch metrics through Prometheus.

APA can use approved live information from this observability path to provide current database-related operational context.

Relevant RDS metrics include:

CPU utilization
Database connections
Free storage
Freeable memory
Read IOPS
Write IOPS
Read latency
Write latency

This allows questions such as:

"What is the current RDS CPU utilization?"

or:

"Are database connections unusually high?"

The data ultimately follows:

RDS
 ↓
CloudWatch
 ↓
YACE
 ↓
Prometheus
 ↓
APA
🌐 ALB Operational Visibility

AFM v3 also monitors the Application Load Balancer through CloudWatch and YACE.

APA can use approved live ALB information to help answer questions involving:

Target health
Request traffic
Response time
Unhealthy targets
HTTP response behavior

The operational path is:

ALB
 ↓
CloudWatch
 ↓
YACE
 ↓
Prometheus
 ↓
APA

This allows APA to correlate information across:

User Traffic
   ↓
ALB
   ↓
Kubernetes Ingress
   ↓
AFM Service
   ↓
Pod
   ↓
RDS
🧠 Hybrid RAG + Live Operations

One of APA's most important capabilities is combining static knowledge with live evidence.

Consider the question:

"The auth-service is failing. What should I check?"

Static RAG can provide:

AFM architecture
Known troubleshooting procedure
Auth-service documentation
Existing runbook
Known dependencies

Live integrations can provide:

Current pod state
Current deployment state
Argo CD status
Current metrics
Current node state
Current AWS information

APA can combine both:

                 User Question
                       │
                       ▼
                      APA
                       │
          ┌────────────┴────────────┐
          │                         │
          ▼                         ▼
      RAG / Docs              Live Platform
          │                         │
          │              ┌──────────┼──────────┐
          │              │          │          │
          │              ▼          ▼          ▼
          │           K8s/EKS    Argo CD   Prometheus
          │
          └────────────┬────────────┘
                       ▼
                  OpenAI API
                       │
                       ▼
             Contextual Response

This is more useful than either static RAG or live monitoring alone.

🧪 Example Operational Questions

APA is intended to answer questions such as:

Kubernetes

"How many pods are running in afm-bank?"

"Which node is running the auth-service pod?"

"Are any AFM pods restarting?"

Argo CD

"Is the auth-service synchronized?"

"Which Git revision is currently deployed?"

AWS

"What is the current state of the EKS cluster?"

"What is the RDS instance status?"

"Are the ALB targets healthy?"

Observability

"What is the current auth-service error rate?"

"Is RDS CPU utilization high?"

"What is the current ALB response time?"

Static Knowledge

"Explain the AFM GitOps architecture."

"How does the Terraform lifecycle work?"

"What is the procedure for troubleshooting CrashLoopBackOff?"

Hybrid

"Auth-service is unhealthy. What is happening and what should I investigate?"

This last category is where APA provides the greatest operational value.

🔍 Evidence-Oriented Responses

APA should distinguish between:

Static Information

Information retrieved from project documentation.

Live Information

Information retrieved from the current platform.

Inference

Reasoning derived from combining multiple pieces of evidence.

This distinction is important because operational state changes continuously.

For example:

Documentation:
"auth-service uses PostgreSQL."


Live Evidence:
"auth-service has restarted 5 times."


Inference:
"The service may be experiencing a runtime failure and
the database should be investigated as one possible dependency."

APA should avoid presenting an inference as a directly observed fact.

🚫 No Autonomous Remediation

APA deliberately stops at information and analysis.

For example, if APA detects:

auth-service
CrashLoopBackOff

it may explain:

Current pod state
Restart count
Recent events
Relevant logs where approved
Deployment status
Argo CD state
Relevant runbook
Possible causes
Recommended investigation steps

It must not automatically:

restart pod
scale deployment
rollback release
patch deployment
sync Argo CD
modify AWS

The engineer remains responsible for deciding and executing any corrective action.

🔐 Security Boundary

APA's security model can be summarized as:

                   User
                     │
                     ▼
                    APA
                     │
          ┌──────────┴──────────┐
          │                     │
          ▼                     ▼
 Kubernetes Read-Only      AWS Read-Only
       RBAC                    IAM
          │                     │
          ▼                     ▼
 Kubernetes API             AWS APIs

Additional integrations such as Argo CD and observability systems are also restricted to approved read-only access.

The security boundary is therefore enforced at the platform authorization layer, not merely through an instruction to the language model.

This is an important design principle:

The model should not be trusted as the security boundary.

Permissions must enforce the boundary independently of what the model is asked to do.

🏃 APA Deployment Architecture

APA follows the same broad application delivery model as the AFM v3 services.

The deployment flow is:

APA Source Repository
        ↓
GitLab CI/CD
        ↓
Build
        ↓
Security Validation
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

This keeps APA deployment aligned with the platform's GitOps architecture.

APA is therefore not manually installed on the EKS cluster.

🐳 Containerization

APA is packaged as a container image.

The image is stored in Amazon ECR and deployed to EKS through GitOps.

This provides:

Immutable application artifacts
Reproducible deployment
Version traceability
Separation between build and runtime
Consistency with the AFM v3 application delivery model

# ☸️ APA EKS Placement
AFM v3 currently uses two dedicated `t3.medium` worker nodes for the
EKS platform.

```text
Amazon EKS
     │
     ├── AFM Worker Node
     │      └── AFM application/platform workloads
     │
     └── APA Worker Node
            └── APA Pod
```

The APA workload runs as a Kubernetes pod in:

afm-platform-assistant

The two-node EKS configuration provides additional scheduling capacity for the AFM platform and APA workload while remaining cost-conscious.

The architecture should not be represented using the obsolete single-node AFM model.
---
## 🎯 APA Node Scheduling and Workload Isolation

APA is intentionally scheduled onto the dedicated APA worker node
using Kubernetes scheduling constraints.

The APA node is configured with:

- **Node label:** `workload=apa`
- **Node taint:** `workload=apa:NoSchedule`

The APA Deployment uses the corresponding:

- **`nodeSelector`:** `workload=apa`
- **Toleration:** `workload=apa:NoSchedule`

This creates an explicit scheduling boundary between the general AFM
workloads and the APA workload.

```text
                 Amazon EKS
                     │
             ┌───────┴────────┐
             │                │
             ▼                ▼
        AFM Worker        APA Worker
          Node              Node
             │                │
             ▼                ▼
      AFM workloads        APA Pod
                              │
                    nodeSelector:
                    workload=apa
                              │
                       toleration:
                    workload=apa:NoSchedule

---

# 🔄 GitOps Deployment

APA deployment state is maintained through the AFM GitOps model.

The flow is:

APA Repository
      ↓
GitLab CI
      ↓
Amazon ECR
      ↓
GitOps Repository
      ↓
Argo CD
      ↓
afm-platform-assistant namespace
      ↓
APA Pod

Argo CD remains responsible for Kubernetes deployment and reconciliation.

APA itself has no authority to modify this deployment process.

# 🧪 APA Validation

APA was validated at multiple levels to confirm application health,
read-only access, live integrations, GitOps visibility, observability
access, and RAG functionality.

Verify:

APA container starts successfully
Kubernetes pod is healthy
Service is reachable
API endpoint responds
Kubernetes Access Validation

Verify that APA can retrieve approved:

Pods
Nodes
Deployments
Services
Namespaces
Workload information

while write operations are denied.

AWS Access Validation

Verify that APA can retrieve approved AWS information using EKS Pod Identity.

Verify that unauthorized write operations are denied by IAM.

Argo CD Validation

Verify that APA can read:

Application state
Sync status
Health status
Revision information

without being able to sync or modify applications.

Observability Validation

Verify that APA can retrieve approved Prometheus/observability information.

RAG Validation

Verify that APA can retrieve relevant static AFM documentation and runbooks.

# 🧪 Read-Only Security Testing

The read-only security boundary is enforced through Kubernetes RBAC and AWS IAM, ensuring that prohibited platform and AWS modifications are outside APA's authorized capabilities.

Examples of prohibited actions to validate:

Restart pod
Scale deployment
Patch deployment
Delete pod
Apply manifest
Sync Argo CD
Modify AWS resource
Create AWS resource
Delete AWS resource

The expected behavior is that these operations are not permitted.

This is an important validation step because APA's safety model depends on actual authorization boundaries rather than prompt instructions alone.

🔗 Relationship with AFM Application Pipelines

APA is not part of the application service pipelines.

The four AFM application services maintain their own repositories and pipelines:

afm-auth-service
afm-login-service
afm-registration-service
afm-frontend-ui

APA is maintained and deployed separately.

The relationship is:

AFM Application Pipelines
        ↓
AFM Services
        ↓
Amazon EKS
        ↓
APA
        ↓
Read-Only Operational Visibility

This keeps application delivery independent from AI-assisted operations.

🔗 Relationship with AFM Observability

The AFM observability platform provides the telemetry that APA can consume.

The observability stack includes:

Prometheus
Grafana
Alertmanager
YACE
AWS CloudWatch
Slack

APA does not replace these systems.

Instead:

Prometheus / Grafana
        ↓
Human Operational Visibility


Prometheus / Approved Live APIs
        ↓
APA
        ↓
Conversational Operational Visibility

Grafana remains the primary dashboard and visualization platform.

APA provides an additional conversational interface.

🔗 Relationship with Argo CD

Argo CD is responsible for GitOps deployment.

APA can inspect Argo CD state but does not control it.

Therefore:

Git
 ↓
Argo CD
 ↓
EKS

is the deployment path.

While:

Argo CD
 ↓
Read-Only APA Integration
 ↓
Operational Information

is the information path.

APA does not become another deployment controller.

🔗 Relationship with Infrastructure

The AFM infrastructure pipeline provisions and manages the AWS platform.

Persistent AWS resources include:

VPC/network foundation
RDS PostgreSQL
ECR
S3
AWS Secrets Manager
Route 53
ACM
ALB-related infrastructure

APA can retrieve approved information about these resources through its read-only AWS integrations.

It does not provision or modify them.

🔄 AFM v3 Operational Architecture

The complete relationship between application delivery, platform infrastructure, observability, and APA is:

                         Developer
                             │
                             ▼
                    AFM Service Repositories
                             │
                             ▼
                        GitLab CI/CD
                             │
                             ▼
                           ECR
                             │
                             ▼
                      GitOps Repository
                             │
                             ▼
                          Argo CD
                             │
                             ▼
                     Amazon EKS Cluster
                  ┌──────────┴──────────┐
                  │                     │
                  ▼                     ▼
          AFM Worker Node        APA Worker Node
                  │                     │
                  ▼                     ▼
            AFM Services             APA Pod
                                        │
                                        ▼
                              Read-Only Integrations
                                        │
              ┌─────────────────────────┼─────────────────────────┐
              │                         │                         │
              ▼                         ▼                         ▼
         Kubernetes                  AWS                    Observability
              │                         │                         │
              │                    EKS Pod Identity              │
              │                         │                         │
              │                    Read-Only IAM                 │
              │                         │                         │
              └─────────────────────────┼─────────────────────────┘
                                        │
                                        ▼
                                  OpenAI API
                                        │
                                        ▼
                              Operational Response
🧱 AFM v3 Layered Architecture

APA sits above the platform as an operational assistance layer.

┌─────────────────────────────────────────────────────┐
│                 User / Engineer                     │
└─────────────────────────┬───────────────────────────┘
                          │
                          ▼
┌─────────────────────────────────────────────────────┐
│              AFM Platform Assistant                 │
│                                                     │
│   RAG + Live Read-Only Operational Evidence         │
└─────────────┬───────────────────────────┬───────────┘
              │                           │
              ▼                           ▼
┌─────────────────────────┐   ┌────────────────────────┐
│ Static Knowledge Layer  │   │ Dynamic Evidence Layer │
│                         │   │                        │
│ ChromaDB / RAG          │   │ Kubernetes / AWS       │
│ Runbooks                │   │ Argo CD / Prometheus   │
│ Architecture            │   │ Grafana / Observability│
│ Project Documentation   │   │                        │
└─────────────────────────┘   └────────────────────────┘
              │                           │
              └─────────────┬─────────────┘
                            ▼
                     OpenAI API
                            │
                            ▼
                  Contextual Response
🧠 Why APA Matters

APA demonstrates the evolution of AFM v3 beyond conventional DevOps automation.

Traditional DevOps tooling provides individual capabilities:

GitLab
Argo CD
Kubernetes
AWS
Prometheus
Grafana

APA adds an operational intelligence layer that can reason across information from these systems.

For example:

Question
   ↓
APA
   ↓
Kubernetes state
+
Argo CD state
+
Prometheus metrics
+
AWS information
+
AFM runbook
   ↓
Contextual operational explanation

This demonstrates the practical application of AI to platform operations without giving the AI unrestricted control over infrastructure.

🏆 Key Engineering Outcomes

The AFM Platform Assistant demonstrates:

AI-assisted platform operations
OpenAI API integration
Static RAG with ChromaDB
Live Kubernetes/EKS integration
Live AWS integration
Argo CD read-only integration
Prometheus/observability integration
RDS operational visibility
ALB operational visibility
Hybrid static + dynamic knowledge
Read-only Kubernetes RBAC
EKS Pod Identity
Dedicated read-only AWS IAM role
No static AWS credentials in the application
Strict prohibition of infrastructure mutations
Kubernetes-native deployment
GitLab CI/CD
Amazon ECR
GitOps deployment
Argo CD reconciliation
Dedicated afm-platform-assistant namespace
Two-node EKS architecture
Dedicated APA node scheduling
Operational troubleshooting assistance
Evidence-oriented responses
Separation between AI reasoning and authorization
🔒 Security Philosophy

The central security principle of APA is:

AI should provide operational intelligence without becoming an infrastructure administrator.

The language model can interpret information.

The integrations can retrieve approved information.

Kubernetes RBAC and AWS IAM enforce authorization.

Therefore, the AI assistant is not trusted with unrestricted infrastructure control.

The resulting model is:

AI Reasoning
     +
Read-Only Evidence
     +
Least-Privilege Authorization
     =
Controlled AI-Assisted Operations

This provides a safer foundation for introducing AI into DevOps and SRE workflows.

🔚 Final Takeaway

AFM Platform Assistant (APA) is a read-only AI operations assistant that combines project knowledge with current platform evidence to help engineers understand and troubleshoot the AFM v3 environment.

APA combines:

RAG + ChromaDB + OpenAI API

with:

Kubernetes/EKS + AWS + Argo CD + Prometheus + Grafana/Observability

to provide a unified operational information layer.

Its deployment follows the same modern platform principles as the rest of AFM v3:

APA Source
   ↓
GitLab CI/CD
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

Its AWS identity follows:

APA Pod
   ↓
Kubernetes ServiceAccount
   ↓
EKS Pod Identity
   ↓
APA Read-Only IAM Role
   ↓
AWS APIs

And its operational boundary remains:

Observe
  ↓
Retrieve
  ↓
Reason
  ↓
Explain

Never:

Modify
Delete
Restart
Scale
Patch
Apply
Sync
Rollback

This makes APA an example of controlled AI-assisted DevOps/SRE operations, where AI improves operational visibility and troubleshooting while Kubernetes RBAC, EKS Pod Identity, and AWS IAM enforce the actual security boundary.
