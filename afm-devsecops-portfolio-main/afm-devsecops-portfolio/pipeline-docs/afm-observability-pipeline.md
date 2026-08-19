# 📊 AFM v3 Observability Pipeline

### Prometheus · Grafana · Alertmanager · YACE · AWS CloudWatch · Argo CD · APA

---

## 🔰 Overview

The **AFM v3 Observability Pipeline** provides centralized monitoring, visualization, alerting, and operational visibility for the AFM platform running on **Amazon EKS**.

In AFM v3, observability is treated as a **GitOps-managed platform capability** rather than a collection of manually installed monitoring components.

The observability stack is maintained in the dedicated **AFM GitOps observability configuration** and deployed and reconciled through **Argo CD**.

AFM v3 also introduces the **AFM Platform Assistant (APA)** as a read-only operational interface that can consume approved live platform telemetry and infrastructure information alongside the existing static AFM knowledge base.

The observability platform provides:

* Application-level monitoring
* Kubernetes and pod-level visibility
* Application Load Balancer monitoring
* Amazon RDS PostgreSQL monitoring
* SRE-oriented operational dashboards
* Prometheus-based metric collection
* PrometheusRule-based alerting
* Alertmanager-based notification routing
* Slack incident notifications
* AWS CloudWatch integration through YACE
* Persistent Prometheus and Grafana storage during the active EKS platform lifecycle
* GitOps reconciliation through Argo CD
* Operational visibility for the AFM Platform Assistant
* Cross-layer troubleshooting capability across application, Kubernetes, and AWS infrastructure

The objective is not simply to display metrics, but to provide the information required to **operate, troubleshoot, validate, and understand the AFM platform**.

---

# 🎯 Observability Design Goals

The AFM v3 observability architecture was designed around the following principles:

* **GitOps-first management**
* **Application and infrastructure visibility**
* **Kubernetes-native monitoring**
* **Selected AWS service monitoring**
* **SRE-oriented operational dashboards**
* **Actionable alerting**
* **Cost-aware operation**
* **Two-node EKS compatibility**
* **Persistent observability storage within the platform lifecycle**
* **Clear separation between metric collection, visualization, alert routing, and operational assistance**
* **Read-only operational visibility for APA**

The design intentionally avoids deploying unnecessary monitoring components.

The goal is:

> **Maximum operational visibility with a minimal and cost-aware observability footprint.**

---

# 🧱 Observability Architecture

AFM v3 uses the following primary observability and operational components:

| Component                                       | Responsibility                                           |
| ----------------------------------------------- | -------------------------------------------------------- |
| **Prometheus**                                  | Kubernetes and AFM application metrics                   |
| **Prometheus Operator / kube-prometheus-stack** | Kubernetes-native Prometheus management                  |
| **Grafana**                                     | Dashboards and operational visualization                 |
| **Alertmanager**                                | Alert routing and Slack notifications                    |
| **YACE**                                        | Export selected AWS CloudWatch metrics for RDS and ALB   |
| **AWS CloudWatch**                              | Source of selected AWS service metrics                   |
| **PrometheusRule**                              | Declarative alert definitions                            |
| **ServiceMonitor**                              | Declarative application metric discovery                 |
| **Argo CD**                                     | GitOps deployment and reconciliation                     |
| **Slack**                                       | Alert notification channel                               |
| **APA**                                         | Read-only operational and platform information interface |

---

# 🔄 High-Level Telemetry Flow

AFM v3 collects telemetry from both the **Kubernetes platform** and selected **AWS services**.

---

## Application / Kubernetes Metrics

```text
AFM Backend Services
        ↓
ServiceMonitor
        ↓
Prometheus
        ↓
Grafana
        ↓
SRE Operations Dashboards
```

Prometheus provides the primary metric store for Kubernetes and application-level telemetry.

Kubernetes-native resources are used to define how application metrics are discovered and monitored.

---

## AWS RDS / ALB Metrics

AFM v3 uses **YACE specifically to bring selected AWS CloudWatch metrics for Amazon RDS PostgreSQL and the Application Load Balancer into Prometheus**.

```text
Amazon RDS PostgreSQL ──┐
                        │
                        ▼
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

The same pattern is used for the Application Load Balancer:

```text
Application Load Balancer
          ↓
   AWS CloudWatch
          ↓
         YACE
          ↓
      Prometheus
          ↓
       Grafana
```

This provides a unified monitoring model where Kubernetes/application metrics and selected AWS service metrics can be viewed through Prometheus and Grafana.

---

## Alerting

```text
Prometheus
     ↓
PrometheusRule
     ↓
Alertmanager
     ↓
Slack
```

Alert definitions are stored as Kubernetes resources and therefore remain part of the GitOps-controlled configuration.

---

## APA Operational Flow

APA is a separate **read-only operations and knowledge layer**.

It can combine approved live platform information with the existing static AFM knowledge/RAG layer to answer operational questions.

Conceptually:

```text
                    ┌──────────────────────┐
                    │   AFM Platform       │
                    │   Assistant (APA)    │
                    └──────────┬───────────┘
                               │
                  Read-only operational access
                               │
              ┌────────────────┼─────────────────┐
              │                │                 │
              ▼                ▼                 ▼
        Kubernetes/EKS     AWS Services      Observability
        Platform State     Information       Information
              │                │                 │
              └────────────────┼─────────────────┘
                               │
                               ▼
                    Operational Answer
```

APA does **not** execute operational mutations.

It does not:

* Restart workloads
* Scale workloads
* Patch Kubernetes resources
* Apply manifests
* Modify AWS resources
* Delete infrastructure
* Sync Argo CD applications
* Change Prometheus/Grafana configuration

Its role is to provide **read-only operational visibility and information**.

---

# 🧩 Application Observability

AFM v3 application observability primarily focuses on the backend AFM services:

* `afm-auth-service`
* `afm-login-service`
* `afm-registration-service`

These services are Spring Boot-based application workloads and provide the primary application-level Prometheus monitoring surface.

Kubernetes-native monitoring resources are used to define the services that Prometheus should scrape.

The application monitoring layer provides visibility into areas such as:

* Request rate
* Error rate
* HTTP status codes
* Request latency
* JVM/application metrics where exposed
* Pod availability
* CPU utilization
* Memory utilization
* Pod restarts
* Service health

---

## Frontend Observability

The `afm-frontend-ui` is monitored differently from the Spring Boot backend services.

Rather than treating the frontend as a backend Prometheus application-metrics endpoint, its operational health is observed through the surrounding platform:

* Kubernetes workload health
* Pod availability
* Resource utilization
* Application Load Balancer behavior
* HTTP response behavior
* End-user request path

This distinction avoids incorrectly representing the React frontend as providing the same application-level metrics interface as the backend services.

---

# ☸️ Kubernetes Observability

Prometheus also monitors the Kubernetes environment supporting AFM.

The EKS environment uses **two worker nodes** during the current AFM v3 platform configuration.

Kubernetes observability provides visibility into:

* Pod availability
* Pod restarts
* Container resource consumption
* Node resource utilization
* Kubernetes workload health
* Scheduling/resource pressure
* Service availability

This is particularly important because application workloads, GitOps components, observability components, and platform add-ons must operate together across the available EKS worker capacity.

The two-node configuration provides additional scheduling capacity while remaining cost-conscious for a portfolio/development environment.

---

# 📡 ServiceMonitor-Based Discovery

AFM v3 uses Kubernetes-native monitoring configuration instead of manually maintaining Prometheus scrape targets.

`ServiceMonitor` resources define the services that Prometheus should monitor.

This provides several advantages:

* Declarative configuration
* Kubernetes-native discovery
* GitOps compatibility
* Reduced manual Prometheus configuration
* Controlled metric collection
* Easier expansion when additional services are introduced

The AFM application namespace is explicitly monitored rather than indiscriminately scraping every workload in the cluster.

---

# ☁️ AWS CloudWatch Integration with YACE

A major improvement in AFM v3 is the integration of selected AWS CloudWatch metrics into the Prometheus/Grafana monitoring model.

**YACE (Yet Another CloudWatch Exporter)** retrieves selected CloudWatch metrics and exposes them in Prometheus-compatible form.

In AFM v3, YACE is specifically used for:

* **Amazon RDS PostgreSQL**
* **Application Load Balancer**

The architecture is:

```text
Selected AWS Resources
        ↓
AWS CloudWatch
        ↓
YACE
        ↓
Prometheus
        ↓
Grafana
```

This avoids maintaining a completely separate monitoring visualization path for these AWS resources.

Instead, selected AWS service telemetry becomes queryable through the same Prometheus/Grafana operational layer used for Kubernetes and application metrics.

---

# 🗄️ Amazon RDS PostgreSQL Monitoring

AFM v3 includes dedicated monitoring for the **Amazon RDS PostgreSQL database**.

YACE is configured to discover and collect selected RDS CloudWatch metrics.

The monitored metrics include:

* `CPUUtilization`
* `DatabaseConnections`
* `FreeStorageSpace`
* `FreeableMemory`
* `ReadIOPS`
* `WriteIOPS`
* `ReadLatency`
* `WriteLatency`

YACE discovery uses AWS resource tags so that the monitoring configuration can identify the intended AFM environment without hardcoding an individual database resource.

For the development environment, discovery is scoped using:

```text
Environment = dev
```

This is important because RDS is a **persistent AWS resource** and exists independently from the ephemeral EKS platform lifecycle.

---

# 🌐 Application Load Balancer Monitoring

AFM v3 also monitors the **AWS Application Load Balancer** serving the application ingress path.

ALB metrics are obtained from CloudWatch through YACE and visualized through Grafana.

The observability layer provides visibility into:

* Request traffic
* Target health
* Response time
* HTTP response behavior
* Unhealthy targets

Dedicated Prometheus alert rules were created for important ALB conditions.

Examples include:

### ALB Unhealthy Targets

Detects unhealthy backend targets and provides early warning of application availability problems.

### ALB High Response Time

Detects elevated load-balancer response time and helps identify latency degradation.

---

# 📈 Grafana

Grafana is the primary visualization layer for AFM v3.

Grafana uses **Prometheus as the primary metrics data source**, allowing application, Kubernetes, RDS, and ALB metrics to be presented through a common operational interface.

The dashboards are designed around operational questions rather than simply displaying available metrics.

---

# 📊 AFM v3 SRE Operations Dashboard

A dedicated dashboard was created for SRE-style application operations.

The dashboard focuses on:

* Availability
* Request rate
* Success rate
* Error rate
* HTTP 4xx errors
* HTTP 5xx errors
* Latency
* p95 latency
* p99 latency
* Service health
* Error-budget-oriented visibility

This allows an operator to answer questions such as:

> Is the application available?

> Is traffic increasing?

> Are requests failing?

> Is latency degrading?

> Which service is experiencing the problem?

---

# 🗄️ RDS PostgreSQL Dashboard

A dedicated RDS dashboard provides database-level visibility.

Important panels include:

* CPU utilization
* Database connections
* Free storage
* Freeable memory
* Read IOPS
* Write IOPS
* Read latency
* Write latency

This helps correlate application incidents with database behavior.

For example:

```text
Application latency increases
        ↓
Check RDS latency
        ↓
Check CPU / connections / IOPS
        ↓
Determine whether database pressure
is contributing to the incident
```

This provides an operational path from an application symptom to a possible database bottleneck.

---

# 🌐 ALB Dashboard

A dedicated ALB dashboard provides visibility into the application's external traffic layer.

Important operational metrics include:

* Request count
* Target response time
* Target health
* Unhealthy targets
* HTTP response behavior
* Traffic patterns

This provides an additional troubleshooting layer between the end user and Kubernetes services.

The operational troubleshooting path can therefore move through:

```text
End User
   ↓
ALB
   ↓
Kubernetes Ingress
   ↓
AFM Service
   ↓
Pod
   ↓
RDS PostgreSQL
```

Each layer can be investigated using the appropriate telemetry source.

---

# 💾 Observability Persistence

AFM v3 explicitly distinguishes between the **ephemeral EKS platform** and the **persistent observability storage used while that platform exists**.

The EKS platform is intentionally destroyed at the end of the development/test cycle to control AWS costs.

However, within the active EKS platform lifecycle:

* Grafana uses persistent storage
* Prometheus uses persistent storage
* Grafana persistence uses **gp3-backed storage**
* Prometheus uses an **8 GiB** persistent volume
* Prometheus retention is configured for **6 hours**

Therefore, observability storage is persistent at the Kubernetes platform level rather than being treated as purely ephemeral pod-local storage.

This means a Grafana or Prometheus pod restart does not inherently mean losing its stored data.

The important lifecycle distinction is:

```text
During active EKS lifecycle
        ↓
Persistent PV / gp3 storage
        ↓
Grafana / Prometheus data survives pod restarts

End-of-day platform teardown
        ↓
EKS platform destroyed
        ↓
Platform-scoped observability storage is not
treated as a permanent cross-cluster archive
```

This design provides the required operational continuity during the working session without turning the development environment into a permanently running monitoring platform.

---

# 🚨 Alerting Architecture

AFM v3 uses **PrometheusRule + Alertmanager** for alerting.

Alert rules are stored declaratively in Git and reconciled through Argo CD.

This makes alert configuration:

* Version controlled
* Reviewable
* Reproducible
* GitOps managed
* Consistent with the desired Kubernetes state

---

# 🔥 AFM Service Availability Alert

A dedicated service availability alert was implemented for the AFM backend services.

The rule monitors Prometheus `up` status for:

```text
auth-service
login-service
registration-service
```

If a monitored service becomes unavailable for the configured evaluation period, the alert enters the firing state.

The alert is classified with:

```text
severity = critical
```

This provides an operational signal when an AFM backend service is no longer being successfully scraped.

---

# 🚨 Infrastructure and Platform Alerts

AFM v3 includes alerting for selected operational conditions that require investigation.

Implemented alert categories include:

* AFM backend service availability
* ALB unhealthy targets
* ALB high response time
* Resource/CPU-related conditions where configured

The alerting model intentionally focuses on meaningful operational conditions rather than generating alerts for every available metric.

This reduces alert noise and keeps the notification channel actionable.

---

# 📢 Slack Alerting

Alertmanager is responsible for routing alerts to **Slack**.

The flow is:

```text
Prometheus
     ↓
PrometheusRule
     ↓
Alertmanager
     ↓
Slack
```

This provides near-real-time notification when important operational conditions occur.

The Slack integration is configured using Kubernetes secrets rather than embedding webhook credentials directly into Git-managed manifests.

---

# 🔐 Observability Security

AFM v3 follows several security principles.

## No Hardcoded Credentials

Secrets such as Slack webhook credentials are not stored directly in Git-managed manifests.

## AWS Permissions

YACE accesses the required AWS CloudWatch APIs through the IAM/workload identity configuration associated with its Kubernetes workload.

Only the permissions required for the configured AWS metric collection should be granted.

## Controlled Metric Discovery

YACE is configured to retrieve selected CloudWatch metrics for the intended RDS and ALB resources rather than collecting arbitrary AWS metrics.

## Kubernetes-Native Permissions

Prometheus, YACE, Grafana, and Alertmanager operate using Kubernetes service accounts and required RBAC permissions.

## Read-Oriented Monitoring

The observability stack primarily reads telemetry and does not perform application or infrastructure mutations.

APA follows the same operational principle: its dynamic integrations are **read-only** and are not permitted to change platform state.

---

# 🧭 GitOps Architecture

Observability configuration is stored separately from application source code.

The AFM GitOps repository contains dedicated observability configuration.

A simplified structure is:

```text
afm-gitops/
└── environments/
    └── dev/
        └── observability/
            ├── kustomization.yaml
            ├── prometheusrule-afm.yaml
            ├── prometheusrule-alb.yaml
            └── ...
```

Argo CD applications manage the platform and observability components.

Relevant applications include:

```text
root-app
platform-app
observability-app
prometheus-stack-app
yace-app
```

These applications have different responsibilities.

The observability-specific applications are responsible for monitoring components, while the broader platform applications provide the surrounding EKS platform capabilities required by the observability stack.

This allows observability components to be reconciled independently while remaining part of the overall AFM GitOps architecture.

---

# 🔄 Argo CD Deployment Flow

The AFM v3 observability deployment follows the GitOps model:

```text
GitLab Repository
       ↓
Observability Git Configuration
       ↓
Argo CD
       ↓
Kubernetes Resources
       ↓
Prometheus / Grafana / Alertmanager / YACE
```

A configuration change is committed to Git.

Argo CD detects the desired-state change and reconciles the Kubernetes cluster.

This eliminates the need for operators to manually reinstall or reconfigure the monitoring stack after configuration changes.

---

# 🧩 Prometheus Operator / CRD Consideration

AFM v3 uses Kubernetes-native Prometheus Operator resources such as:

* `ServiceMonitor`
* `PrometheusRule`

During implementation, the Prometheus Operator CRD lifecycle required special handling because Argo CD encountered metadata annotation-size limitations while attempting to manage certain CRDs.

The solution was to:

* Install the required Prometheus Operator CRDs separately
* Configure the Prometheus stack deployment with `skipCrds: true`
* Use Argo CD synchronization options including:

  * `ServerSideApply`
  * `CreateNamespace`

This allowed the monitoring stack to be deployed reliably through GitOps without repeatedly attempting to recreate the problematic CRDs.

This is an example of a real Kubernetes/GitOps implementation issue encountered and resolved during AFM v3 development.

---

# ⚙️ Two-Node Resource-Constrained Design

AFM v3 currently operates on a **two-node EKS worker configuration**.

The observability stack must coexist with:

* AFM application workloads
* Argo CD
* AWS Load Balancer Controller
* YACE
* Prometheus
* Grafana
* Alertmanager
* Other platform components

Resource consumption was therefore considered when configuring:

* Prometheus replicas
* Grafana replicas
* Prometheus retention
* Persistent storage
* Dashboard complexity
* Supporting exporters

The observability stack uses a **single-replica model** appropriate for the development/portfolio environment.

This should not be interpreted as a production high-availability architecture.

Instead, it demonstrates practical:

* Cost management
* Resource planning
* Capacity awareness
* Reliability trade-offs

---

# 🧪 Observability Validation

Observability is validated as an operational capability rather than simply checking whether monitoring pods are running.

Validation includes:

## Prometheus

* Prometheus is healthy
* Targets are discovered
* Metrics are available
* AFM services are visible

## Grafana

* Datasources are working
* Dashboards load correctly
* Application metrics are queryable
* RDS metrics are queryable
* ALB metrics are queryable

## YACE

* AWS resources are discovered
* CloudWatch metrics are exported
* RDS metrics are available
* ALB metrics are available

## Alertmanager

* Alert rules are evaluated
* Alerts enter the firing state when conditions are simulated
* Alertmanager receives firing alerts
* Slack notification delivery is verified

## APA

* Read-only platform information can be retrieved
* Approved live integrations provide current operational information
* APA does not perform infrastructure mutations
* Static knowledge/RAG information can complement live operational data

---

# 🧪 Real Alert Testing

AFM v3 observability was tested using controlled failure and metric scenarios rather than relying only on configuration validation.

For example, a temporary CPU-intensive workload was used to generate resource pressure and verify that the monitoring and alerting path behaved as expected.

This validates the complete operational chain:

```text
Condition
   ↓
Metric
   ↓
Prometheus
   ↓
Alert Rule
   ↓
Alertmanager
   ↓
Slack
```

This distinction is important:

> **A monitoring stack is not considered complete merely because Grafana displays metrics. The alerting path must also be tested end-to-end.**

---

# 🔁 Relationship with the AFM Platform Lifecycle

AFM v3 intentionally separates AWS resources according to their lifecycle.

## Ephemeral / Recreated

The following platform components can be destroyed and recreated during development:

* EKS cluster
* Kubernetes platform components
* EKS add-ons
* Cluster base components
* GitOps-deployed observability workloads
* APA runtime deployed on the EKS platform

These components form the **ephemeral application/platform layer**.

---

## Persistent / Build Once

The following AWS resources are treated separately:

* VPC/network foundation
* RDS PostgreSQL
* ECR repositories
* S3 Terraform state bucket
* S3 ALB log bucket
* AWS Secrets Manager secrets
* Route 53 configuration
* ACM certificates
* Other persistent AWS dependencies

These resources are not destroyed as part of the normal EKS platform teardown.

---

## Lifecycle Model

The overall lifecycle can therefore be represented as:

```text
                 AFM v3 AWS Foundation
                         │
          ┌──────────────┴──────────────┐
          │                             │
          ▼                             ▼
 Persistent AWS Resources       Ephemeral EKS Platform
          │                             │
          │                    ┌────────┴────────┐
          │                    │                 │
          │                    ▼                 ▼
          │               Applications     Observability
          │                                    │
          │                                    ▼
          │                                    APA
          │
          └─────────────── remains available
                           across EKS
                           recreation
```

Destroying the EKS platform therefore does **not** imply destroying the underlying application data or persistent AWS service dependencies.

The observability architecture is designed with this lifecycle model in mind.

---

# 🏗️ AFM v3 End-to-End Observability Model

The resulting monitoring architecture can be summarized as:

```text
                    ┌───────────────────────┐
                    │      AFM Services     │
                    │ auth / login / reg.   │
                    └───────────┬───────────┘
                                │
                         ServiceMonitor
                                │
                                ▼
                         ┌─────────────┐
                         │ Prometheus  │
                         └──────┬──────┘
                                │
              ┌─────────────────┼─────────────────┐
              │                 │                 │
              ▼                 ▼                 ▼
          Grafana          Alert Rules      Kubernetes
              │                 │             Metrics
              │                 ▼
              │           Alertmanager
              │                 │
              │                 ▼
              │               Slack
              │
              ▼
       SRE Operations
          Dashboard


      AWS RDS PostgreSQL
              │
              ▼
       AWS CloudWatch
              │
              ▼
             YACE
              │
              ▼
         Prometheus
              │
              ├──────────────► Grafana
              │
              └──────────────► RDS Dashboard


     Application Load Balancer
              │
              ▼
       AWS CloudWatch
              │
              ▼
             YACE
              │
              ▼
         Prometheus
              │
              ├──────────────► Grafana
              │
              └──────────────► ALB Dashboard


                 Git Repository
                       │
                       ▼
                    Argo CD
                       │
                       ▼
              Kubernetes Observability


                 ┌───────────────┐
                 │      APA      │
                 │ Read-Only Ops │
                 └───────┬───────┘
                         │
            ┌────────────┼────────────┐
            │            │            │
            ▼            ▼            ▼
       Kubernetes       AWS       Observability
       Information   Information   Information
```

---

# 🧠 Why AFM v3 Observability Matters

The observability implementation demonstrates more than installing Prometheus and Grafana.

It demonstrates the ability to design an operational monitoring system across multiple layers.

## Application Layer

AFM backend service health, request behavior, latency, errors, and resource consumption.

## Kubernetes Layer

Pods, workloads, nodes, scheduling pressure, resource utilization, and platform health.

## AWS Layer

Selected RDS PostgreSQL and ALB metrics obtained from CloudWatch through YACE.

## SRE Layer

Availability, request rate, success rate, error rate, latency, p95/p99 behavior, and operational dashboards.

## Incident Layer

Prometheus alert rules, Alertmanager routing, and Slack notification.

## GitOps Layer

Declarative observability configuration reconciled by Argo CD.

## AI Operations Layer

APA consumes approved read-only platform, AWS, Kubernetes, and observability information and presents it through an operational assistance interface.

---

# 🏆 Key Engineering Outcomes

The AFM v3 observability implementation demonstrates:

* **GitOps-managed observability**
* **Prometheus-based metric collection**
* **Kubernetes-native monitoring with Prometheus Operator**
* **ServiceMonitor-based service discovery**
* **PrometheusRule-based alerting**
* **Alertmanager-based notification routing**
* **Slack incident notifications**
* **YACE-based CloudWatch integration**
* **RDS PostgreSQL monitoring**
* **Application Load Balancer monitoring**
* **SRE-oriented Grafana dashboards**
* **Persistent Grafana storage**
* **Persistent Prometheus storage**
* **Controlled Prometheus retention**
* **Two-node resource-aware EKS deployment**
* **End-to-end alert testing**
* **Operational troubleshooting capability**
* **GitOps-managed monitoring configuration**
* **Read-only AI-assisted operational visibility through APA**
* **Clear separation between ephemeral platform resources and persistent AWS dependencies**

---

# 🔚 Final Takeaway

> **AFM v3 treats observability as a GitOps-managed operational platform rather than a collection of monitoring tools.**

Prometheus provides the central metrics layer, Grafana provides operational visualization, Alertmanager handles alert routing, YACE bridges selected AWS CloudWatch metrics for **RDS PostgreSQL and ALB** into Prometheus, and Argo CD ensures the observability configuration remains declarative and reproducible.

The **AFM Platform Assistant (APA)** extends this operational model by providing a read-only interface for understanding current platform, AWS, Kubernetes, and observability state without allowing the assistant to modify infrastructure.

The resulting operational model covers:

**AFM Applications → Kubernetes → ALB → RDS → AWS CloudWatch**

with:

**Prometheus → Grafana → Alertmanager → Slack**

and:

**Git → Argo CD → Kubernetes**

while:

**APA → Read-only operational visibility**

sits above these systems as an operational assistance layer.

The platform remains intentionally cost-conscious: the **EKS environment and its workloads are ephemeral and can be destroyed at the end of the development cycle**, while persistent AWS foundation resources such as RDS, ECR, S3, Secrets Manager, Route 53, and ACM remain available for subsequent platform recreation.

This makes AFM v3 observability representative of **practical DevOps, SRE, GitOps, cloud operations, and AI-assisted operations engineering**, rather than simply a dashboard demonstration.
