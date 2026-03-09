# Project: Cloud-Native Microservices Re-Engineering
**Objective:** Transform a complex, multi-language microservices application into a production-ready system using IaC, GitOps, and automated CI/CD.

## Phase 0: Project Initiation & Workspace Setup
- Initialize Git repository with a clean directory structure (`/terraform`, `/k8s`, `/src`, `/docs`).
- Configure Git identity and verify `.gitignore` for Java, Python, and Go.
- Create a high-level Architecture Diagram (Hand-drawn or Excalidraw).
- Map out environment variables and secret management strategy.

## Phase 1: Local Development & Containerization
- Install and configure Docker on Fedora/EC2.
- Write custom Multi-stage Dockerfiles for Java, Go, and Python services.
- Orchestrate locally using Docker Compose to verify service-to-service communication.
- Optimize image sizes using Alpine/Distroless bases.

## Phase 2: Infrastructure as Code (IaC) with Terraform
- Configure AWS CLI and IAM permissions.
- Setup Terraform S3 Backend with DynamoDB state locking.
- Provision VPC, Subnets, and Security Groups.
- Deploy an EKS (Elastic Kubernetes Service) cluster and Managed Node Groups.

## Phase 3: Kubernetes Orchestration & Networking
- Connect `kubectl` to the EKS cluster.
- Write and apply Kubernetes Manifests (Deployments, Services).
- Implement Storage Classes, Persistent Volumes (PV), and Claims (PVC) for databases.
- Deploy an Ingress Controller (Nginx/ALB) for external traffic management.

## Phase 4: Domain & Traffic Management
- Setup AWS Route53 Hosted Zones.
- Configure Custom Domain and link it to the Ingress Load Balancer.
- Verify end-to-end traffic flow from the public URL to the backend services.

## Phase 5: Continuous Integration (CI) with GitHub Actions
- Create GitHub Actions workflows for automated testing and building.
- Implement security scanning (Trivy/Snyk) within the pipeline.
- Automate image tagging and pushing to Docker Hub or ECR.

## Phase 6: Continuous Delivery (CD) & GitOps with Argo CD
- Install Argo CD in the EKS cluster.
- Connect Argo CD to the Git repository for automated syncing.
- Implement a "Pull-based" deployment strategy (GitOpsification).
- Verify end-to-end CI/CD (Commit -> Build -> Auto-Deploy).

## Phase 7: Observability & Final Presentation
- Deploy OpenTelemetry, Prometheus, and Grafana.
- Create custom dashboards for microservice health and latency.
- Finalize README.md with "Technical Challenges Solved" and "Before/After" metrics.