# Cloud-Native Microservices Infrastructure Re-Engineering
**Architect:** Divya Anand | **Project Type:** DevOps / SRE Portfolio

## 📌 Project Overview
This project involves the complete infrastructure re-engineering of a high-complexity, polyglot microservices application. While the base application logic is derived from a distributed microservices demo, the **core contribution of this project is the design and implementation of a production-grade DevOps lifecycle.** I stripped out all existing deployment configurations to architect a fresh, hardened environment featuring Infrastructure as Code (IaC), GitOps continuous delivery, and cross-runtime observability.

## 🏗 Service Architecture
The system consists of 10+ microservices communicating via **gRPC** and **HTTP**, utilizing a mix of Java, Go, Python, and Node.js.

![Service Architecture Diagram](docs/Architecture.png)

### 🛠 Technical Pillars
* **Infrastructure:** AWS (EKS, VPC, Route53, S3) provisioned via **Terraform**.
* **Containerization:** Custom multi-stage builds focused on image size reduction and security.
* **Deployment:** GitOps workflow managed by **Argo CD** for automated cluster synchronization.
* **Monitoring:** Distributed tracing and metrics using **OpenTelemetry**, Prometheus, and Grafana.

---

## 🚀 Execution Roadmap

### Phase 0: Project Initiation & Workspace Setup
- Engineered a clean repository structure to separate infrastructure from source code.
- Mapped service dependencies (gRPC/HTTP) to define networking and security group rules.

### Phase 1: Custom Containerization & Optimization
- **Rebuilt Dockerfiles:** Replaced generic images with custom multi-stage builds.
  - Reduced **Java** image footprints by utilizing OpenJDK-Alpine.
  - Secured **Go** and **Python** services using Distroless bases to eliminate OS-level vulnerabilities.
- Validated service discovery locally using a customized Docker Compose environment.

### Phase 2: Infrastructure as Code (IaC) with Terraform
- Provisioned a modular **VPC** with public/private subnet isolation for database security.
- Implemented **Remote State Management** with S3 and DynamoDB for state locking.
- Deployed a highly available **Amazon EKS** cluster with managed node groups.

### Phase 3: Kubernetes Orchestration & Persistence
- Authored native Kubernetes Manifests (Deployments, Services, ConfigMaps).
- Configured **EBS-backed Storage Classes** for persistent state management (PostgreSQL/Redis).
- Implemented an **Nginx Ingress Controller** for intelligent path-based routing.

### Phase 4: CI/CD & GitOps Integration
- **CI (GitHub Actions):** Developed pipelines for automated linting, security scanning (Trivy), and image pushing.
- **CD (Argo CD):** Implemented a GitOps "Pull" model. The EKS cluster monitors this repository and auto-updates whenever changes are pushed to the `/k8s` directory.

### Phase 5: Full-Stack Observability
- Integrated **OpenTelemetry** to provide end-to-end visibility across the polyglot stack.
- Configured custom Grafana dashboards to monitor gRPC latencies and pod resource utilization.

---

## 💡 Key Engineering Achievements
- **Infrastructure Automation:** Reduced environment spin-up time from hours to minutes using Terraform.
- **Security Hardening:** Implemented non-root container users and scanned all images for vulnerabilities.
- **Self-Healing Cluster:** leveraged Argo CD to ensure zero-drift between Git and Production.