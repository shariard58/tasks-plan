# Backend Phase 13 — DevOps, Docker, CI/CD & Cloud (Tasks 411-445)

> **Code লেখা শেষ না — code deploy করা পর্যন্ত কাজ। DevOps না জানলে তুমি half developer।**
> **Docker, Kubernetes, CI/CD, AWS — production deploy ও manage করতে পারলে তুমি complete।**
> **Senior devs শুধু code লেখে না — infrastructure ও manage করে।**

---

## Section A: Docker Mastery (Tasks 411-425)

### Task 411 — Docker Fundamentals 🟢

- Containers vs VMs, Images, Containers, Layers, Registries
- **Exercise:** Run Node.js app in Docker, understand image layers

### Task 412 — Dockerfile Best Practices 🟡

- Multi-stage builds, layer caching, .dockerignore, non-root user, minimal images
- **Exercise:** Optimized Dockerfile: production image < 100MB

### Task 413 — Docker Compose 🟡

- Multi-container applications: app + database + redis + nginx
- Networking, volumes, depends_on, health checks, environment variables
- **Exercise:** Full stack Docker Compose (Node.js + PostgreSQL + Redis + Nginx)

### Task 414 — Docker Networking 🟡

- Bridge, Host, Overlay, None networks
- Container-to-container communication, DNS resolution
- **Exercise:** Multi-container networking with custom networks

### Task 415 — Docker Volumes & Data Persistence 🟡

- Named volumes, bind mounts, tmpfs
- Database data persistence, log storage
- **Exercise:** PostgreSQL with persistent volume (data survives container restart)

### Task 416 — Docker Security 🟡

- Non-root user, read-only filesystem, resource limits, image scanning
- **Exercise:** Secure Docker setup (least privilege, scanned, limited)

### Task 417 — Docker Registry 🟢

- Docker Hub, GitHub Container Registry, private registry
- **Exercise:** Push image to registry, pull ও run on different machine

### Task 418 — Docker for Development 🟡

- Hot reload in Docker, debugging containerized apps, VS Code Dev Containers
- **Exercise:** Development environment with hot reload in Docker

### Task 419 — Docker Multi-Stage Builds 🟡

- Build stage (npm install, build) → Production stage (only runtime)
- **Exercise:** Multi-stage build for TypeScript Node.js app

### Task 420 — Docker Monitoring & Logging 🟡

- Docker stats, logs, health checks
- Log drivers (json-file, syslog, fluentd)
- **Exercise:** Container monitoring setup

### Task 421 — Docker Troubleshooting 🟡

- Debug container issues: exec, logs, inspect, events
- Common issues: port conflicts, network, volume permissions, OOM
- **Exercise:** 10 Docker debugging scenarios

---

## Section B: CI/CD (Tasks 422-432)

### Task 422 — CI/CD Concepts 🟢

- Continuous Integration: auto-test on every commit
- Continuous Delivery: auto-deploy to staging
- Continuous Deployment: auto-deploy to production
- **Exercise:** Document CI/CD pipeline design

### Task 423 — GitHub Actions 🟡

- Workflow files, jobs, steps, actions, secrets, environments
- **Exercise:** CI pipeline: lint → test → build → deploy

### Task 424 — Testing in CI 🟡

- Run unit tests, integration tests, E2E tests in pipeline
- Parallel test execution, test reporting
- **Exercise:** Full test pipeline with coverage reporting

### Task 425 — Docker in CI/CD 🟡

- Build Docker image → Test → Push to registry → Deploy
- **Exercise:** Docker-based CI/CD pipeline

### Task 426 — Database Migrations in CI/CD 🟡

- Auto-run migrations on deploy, rollback strategy
- **Exercise:** Migration step in deployment pipeline

### Task 427 — Blue-Green Deployment 🟡

- Two identical environments: switch traffic between them
- Zero-downtime deployment
- **Exercise:** Blue-green deployment strategy

### Task 428 — Canary Deployment 🟡

- Roll out to small percentage, monitor, gradually increase
- **Exercise:** Canary release: 5% → 25% → 50% → 100%

### Task 429 — Rolling Deployment 🟢

- Replace instances one by one, zero downtime
- **Exercise:** Rolling deployment with health checks

### Task 430 — Rollback Strategy 🟡

- Automatic rollback on failure, database rollback
- **Exercise:** Automated rollback on health check failure

### Task 431 — Feature Flags in Deployment 🟡

- Deploy code without activating features
- **Exercise:** Feature flag controlled deployment

### Task 432 — GitOps 🟡

- Git as single source of truth for infrastructure
- ArgoCD, Flux concepts
- **Exercise:** GitOps workflow design

---

## Section C: Cloud & Infrastructure (Tasks 433-445)

### Task 433 — Cloud Computing Fundamentals 🟢

- IaaS, PaaS, SaaS, FaaS (Serverless)
- AWS, GCP, Azure comparison
- **Exercise:** Cloud service mapping document

### Task 434 — AWS Core Services 🟡

- EC2, RDS, S3, Lambda, CloudFront, Route53, SQS, SNS, ElastiCache
- **Exercise:** Deploy app on AWS (EC2 + RDS + S3 + CloudFront)

### Task 435 — AWS Lambda & Serverless 🟡

- Functions as service, API Gateway + Lambda, cold starts
- **Exercise:** Serverless API (Lambda + API Gateway + DynamoDB)

### Task 436 — Infrastructure as Code (Terraform) 🟡

- Declarative infrastructure: define ও provision cloud resources
- **Exercise:** Terraform: provision EC2 + RDS + Redis + S3

### Task 437 — Kubernetes Fundamentals 🔴

- Pods, Deployments, Services, Ingress, ConfigMaps, Secrets, Namespaces
- **Exercise:** Deploy app to Kubernetes (local with minikube/kind)

### Task 438 — Kubernetes for Node.js 🟡

- Deployment YAML, Service, Ingress, Health probes, Resource limits
- **Exercise:** Full K8s deployment for Node.js app

### Task 439 — Kubernetes Scaling 🟡

- Horizontal Pod Autoscaler (HPA), Vertical Pod Autoscaler
- **Exercise:** Auto-scaling based on CPU/custom metrics

### Task 440 — Helm Charts 🟡

- Package manager for Kubernetes
- **Exercise:** Helm chart for your application

### Task 441 — Secrets Management in Cloud 🟡

- AWS Secrets Manager, K8s Secrets, Vault
- **Exercise:** Secrets rotation ও injection into containers

### Task 442 — SSL/TLS in Production 🟢

- Let's Encrypt, cert-manager (K8s), AWS ACM
- **Exercise:** Automated SSL certificate management

### Task 443 — DNS & Domain Management 🟢

- Route53 / Cloudflare, A records, CNAME, health checks
- **Exercise:** Domain setup with DNS ও health checks

### Task 444 — Cost Optimization 🟢

- Right-sizing, Reserved instances, Spot instances, Auto-scaling
- **Exercise:** Cloud cost analysis ও optimization plan

### Task 445 — Build: Complete Deployment Pipeline ⚫

- **Phase 13 Final Project:**
  - Docker: Multi-stage build, optimized image
  - Docker Compose: local development
  - CI/CD: GitHub Actions (lint → test → build → push → deploy)
  - Cloud: AWS deployment (EC2/ECS + RDS + Redis + S3)
  - Monitoring: CloudWatch / Prometheus
  - SSL: Let's Encrypt auto-renewal
  - Blue-green or canary deployment
  - Automated rollback on failure
  - Infrastructure as Code (Terraform or CloudFormation)

---

## ✅ Phase 13 Completion Checklist

- [ ] সব 35 tasks complete
- [ ] Docker image build, run, compose করতে পারো
- [ ] CI/CD pipeline setup করতে পারো (GitHub Actions)
- [ ] AWS core services use করতে পারো
- [ ] Kubernetes basics (deploy, scale, manage)
- [ ] Infrastructure as Code (Terraform basics)
- [ ] Zero-downtime deployment strategies জানো

> _"Code লেখা = 50%। Deploy + manage = বাকি 50%। দুটো পারলে তুমি complete।"_
