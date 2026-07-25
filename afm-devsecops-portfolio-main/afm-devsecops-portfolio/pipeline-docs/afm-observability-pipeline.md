# 📊 AFM Observability Pipeline

### Prometheus · Grafana · CloudWatch via GitLab CI/CD

---
## 🔰 Overview
The **AFM Observability Pipeline** is responsible for deploying, configuring, and validating **monitoring and alerting** for the AFM platform running on Amazon EKS.

Rather than installing monitoring tools manually, observability is treated as a **first-class CI/CD concern**, deployed and managed via a **dedicated GitLab pipeline**.

The pipeline enables:
- Application-level visibility
- Infrastructure-level visibility
- Alerting for failures and bottlenecks
- Debugging under constrained resources

This reflects how **real DevOps and SRE teams** operationalize observability.

---
## 🎯 Observability Design Goals
The observability setup was designed to:
- Separate **application metrics** from **infrastructure metrics**
- Work reliably on a **single-node EKS cluster**
- Avoid unnecessary complexity
- Provide actionable dashboards and alerts
- Integrate cleanly with existing CI/CD workflows

The goal was **operational clarity**, not tool sprawl.

---

## 🧱 Tools & Responsibilities

|Tool|Responsibility|
|---|---|
|**Prometheus**|Application & Kubernetes metrics|
|**Grafana**|Visualization, dashboards, alerting|
|**CloudWatch Agent**|Node-level infrastructure metrics|
|**AWS CloudWatch**|Metric storage & infra monitoring|
|**Slack**|Alert notifications|

Each tool has a **clearly defined scope**, avoiding overlap.

---
## 🧩 Metric Separation Strategy

### Application Metrics (AFM Services)
Collected using **Prometheus**:

- HTTP request count
- Request latency
- Error rates
- JVM metrics (Spring Boot)
- Pod-level resource usage

Flow:
`AFM Pods → Prometheus → Grafana`

---
### Infrastructure Metrics (EKS Nodes)
Collected using **CloudWatch Agent (DaemonSet)**:
- Node CPU usage
- Memory utilization
- Disk usage
- Network I/O

Flow:

`EKS Node → CloudWatch Agent → AWS CloudWatch → Grafana`

> CloudWatch Agent runs as a **DaemonSet**, implicitly covering all worker nodes without explicitly modeling nodes in diagrams.

---

## 🚦 Observability Pipeline Structure (GitLab CI/CD)
Observability is deployed using a **separate pipeline**, keeping it isolated from:
- Infrastructure provisioning
- Application CI/CD

This prevents accidental coupling and reduces blast radius.

---
## 🛠️ Pipeline Stages
### 1️⃣ Prepare & Validate
- Verifies cluster access
- Confirms namespace availability    
- Validates permissions for metric components

Prevents partial or broken observability deployments.

---
### 2️⃣ Prometheus Installation
- Deploys Prometheus into the cluster
- Configured to scrape:
    - Kubernetes APIs
    - AFM application metrics endpoints
- Minimal footprint to suit single-node constraints
---

### 3️⃣ ServiceMonitor Configuration
- Defines **ServiceMonitor** resources
- Explicitly targets AFM services in the `afm-bank` namespace
- Ensures Prometheus scrapes only intended services

This avoids noisy or unnecessary metrics.

---

### 4️⃣ Grafana Installation & Configuration
- Deploys Grafana
- Configures multiple data sources:
    - Prometheus (application metrics)
    - CloudWatch (infrastructure metrics)
- Imports dashboards for:
    - AFM service health
    - Pod behavior
    - Node pressure

---
### 5️⃣ CloudWatch Agent Deployment
- Deploys CloudWatch Agent as a **DaemonSet**
- Collects node-level metrics only
- Sends metrics to AWS CloudWatch    

No application metrics are duplicated here.

---
### 6️⃣ Verification & Validation
- Confirms metrics availability in Grafana
- Validates dashboard rendering
- Verifies alert thresholds trigger correctly

This ensures observability is **operational**, not just installed.

---
## 🚨 Alerting Strategy
Alerts are configured in **Grafana**, not Prometheus directly, to centralize notification logic.
### Alert Categories
- Pod restarts
- High CPU / memory usage
- Disk pressure    
- Service latency
- Node saturation

### Notification Channel

- **Slack** is used for alert delivery
- Enables rapid visibility of failures during testing and simulation

---
## ⚠️ Real Challenges Encountered
### Single-Node Scheduling Pressure (t3.medium)
- Initial observability stack caused:
    - Pod scheduling failures
    - Resource starvation

### Resolution
- Reduced replica counts
- Tuned Prometheus resource limits
- Simplified dashboards
- Removed non-essential exporters
This reinforced **cost-aware and constraint-driven design**.

---

## 🔐 Security Considerations
- No credentials hardcoded in manifests
- AWS permissions handled via IAM roles
- Metrics access scoped to required namespaces only
- Grafana access limited to internal testing

---
## 🔁 Integration with AFM Platform
- Observability pipeline runs **after infrastructure provisioning**
- AFM application pipelines depend on observability being available
- No infrastructure resources are modified by this pipeline

This clean separation mirrors **real production workflows**.

---
## 🧠 Why This Pipeline Matters
This observability pipeline demonstrates:
- Practical monitoring under constraints
- Separation of app vs infra telemetry
- CI/CD-driven operations
- Debugging-oriented design
- SRE-style thinking

It is **not a dashboard demo**, but an **operational system**.

---
## 🔚 Final Takeaway

> Observability in AFM is treated as an operational requirement, not an afterthought.

By deploying monitoring via CI/CD and validating it under real constraints, the AFM project demonstrates how **production DevOps platforms are monitored, debugged, and stabilized**.