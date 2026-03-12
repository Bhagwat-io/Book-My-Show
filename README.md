# 🎬 BookMyShow App — DevOps Capstone Project

> **End-to-end CI/CD pipeline automation** for the BookMyShow application using GitHub, Jenkins, Docker, Kubernetes (EKS), and Monitoring with Prometheus & Grafana.

**Submitted by:** Bhagwat Kolekar

---

## 📋 Table of Contents

- [Project Overview](#project-overview)
- [Tech Stack](#tech-stack)
- [Architecture](#architecture)
- [Project Phases](#project-phases)
  - [1. Jira Workflow](#1-jira-workflow)
  - [2. GitHub Workflow](#2-github-workflow)
  - [3. Jenkins CI/CD Pipeline](#3-jenkins-cicd-pipeline)
  - [4. Docker Deployment](#4-docker-deployment)
  - [5. Kubernetes Deployment (EKS)](#5-kubernetes-deployment-eks)
  - [6. Monitoring & Observability](#6-monitoring--observability)
- [Key Deliverables](#key-deliverables)
- [Getting Started](#getting-started)

---

## Project Overview

This capstone project demonstrates a fully automated DevOps pipeline for the BookMyShow web application. Starting from source control management through to production monitoring, every stage is automated and observable.

**Goal:** Automate the CI/CD pipeline for the BookMyShow React application — from code commit to container deployment — with full monitoring and alerting.

---

## Tech Stack

| Category | Tools |
|---|---|
| Project Management | Jira |
| Source Control | GitHub |
| CI/CD | Jenkins |
| Code Quality | SonarQube |
| Containerization | Docker, DockerHub |
| Orchestration | Kubernetes (AWS EKS) |
| Monitoring | Prometheus, Grafana |
| Alerting | Grafana Alerts → Slack |
| Cloud | AWS EC2, EKS |

---

## Architecture

```
Developer Push
      │
      ▼
GitHub (feature branch → PR → main)
      │
      ▼
Jenkins Pipeline
  ├── Clean Workspace
  ├── Checkout Code
  ├── SonarQube Analysis (Quality Gate)
  ├── Install Dependencies (npm)
  ├── Docker Build & Push → DockerHub
  ├── Deploy to Docker Container
  └── Email Notification
      │
      ▼
Kubernetes (EKS)
  ├── Namespace: bhagwat
  ├── Deployment (2 replicas)
  └── Service (LoadBalancer → Port 80)
      │
      ▼
Monitoring Stack (monitoring namespace)
  ├── Prometheus (metrics scraping)
  ├── Node Exporter (system metrics)
  └── Grafana (dashboards + alerts → Slack)
```

---

## Project Phases

### 1. Jira Workflow

Tasks were created and tracked in Jira using a Kanban board with the following workflow:

```
To Do → In Progress → In Review → Done
```

**Tasks tracked:**
- `BMSP-1` Jira Workflow
- `BMSP-2` GitHub Workflow
- `BMSP-3` Jenkins CI/CD Pipeline
- `BMSP-4` Docker Deployment
- `BMSP-5` Kubernetes Deployment (EKS)
- `BMSP-6` Monitoring & Observability

All tasks were self-assigned with start dates and due dates configured.

---

### 2. GitHub Workflow

**Repository (forked from):** `https://github.com/akshu20791/Book-My-Show/`

**Personal fork:** `https://github.com/Bhagwat-io/Book-My-Show.git`

**Steps followed:**
1. Forked the upstream repository to personal GitHub account
2. Cloned the fork locally
3. Created a `feature` branch
4. Made changes (added K8s YAML files and Jenkinsfile)
5. Committed and pushed to feature branch
6. Raised a Pull Request — `feature → main`
7. Reviewed, merged, and confirmed changes reflected in `main`

**Pull Request:** [https://github.com/Bhagwat-io/Book-My-Show/pull/1](https://github.com/Bhagwat-io/Book-My-Show/pull/1)

---

### 3. Jenkins CI/CD Pipeline

**Infrastructure:** Ubuntu EC2 instances on AWS
- `Bhagwat-Jenkins-SonarQube` — `t2.medium` (Jenkins on port `8080`, SonarQube on port `9000`)
- `Bhagwat-docker` — `t2.medium` (Docker container host)

**Jenkins Plugins Installed:**
- Eclipse Temurin Installer
- SonarQube Scanner
- NodeJS
- Docker, Docker Commons, Docker Pipeline, Docker API, docker-build-step
- Pipeline Stage View
- Email Extension Template
- Kubernetes, Kubernetes CLI, Kubernetes Client API, Kubernetes Credentials
- Config File Provider


**Credentials configured in Jenkins:**
| ID | Type | Purpose |
|---|---|---|
| `sonar-token` | Secret Text | SonarQube authentication |
| `docker-token` | Username/Password | DockerHub push access |
| `email-cred` | Username/Password | Gmail SMTP notifications |

**SonarQube Results:**
- Quality Gate: ✅ **Passed**
- Bugs: 18 | Vulnerabilities: 0 | Security Hotspots: 3
- Maintainability: A | Security: A | Reliability: C

---

### 4. Docker Deployment

**DockerHub Image:** [https://hub.docker.com/repository/docker/bhagwat07/bms-app/tags](https://hub.docker.com/repository/docker/bhagwat07/bms-app/tags)

**Docker run command:**
```bash
docker run -d --restart=always --name bms-app -p 3000:3000 bhagwat07/bms-app:v1.0.0
```

Application accessible at: `http://<EC2-PUBLIC-IP>:3000`

---

### 5. Kubernetes Deployment (EKS)

**Prerequisites installed on cluster VM:**
```bash
# kubectl
curl -LO "https://dl.k8s.io/release/$(curl -sL https://dl.k8s.io/release/stable.txt)/bin/linux/amd64/kubectl"

# eksctl
curl -sLO "https://github.com/eksctl-io/eksctl/releases/latest/download/eksctl_$(uname -s)_$PLATFORM.tar.gz"

# AWS CLI
curl "https://awscli.amazonaws.com/awscli-exe-linux-x86_64.zip" -o "awscliv2.zip"
```

**Cluster creation:**
```bash
eksctl create cluster \
  --name bhagwat-cluster \
  --region us-east-2 \
  --zones us-east-2a,us-east-2b \
  --nodegroup-name k8s-ng \
  --node-type t2.medium \
  --nodes 2
```


**Apply manifests:**
```bash
kubectl create ns bhagwat
kubectl apply -f service.yml
kubectl apply -f deployment.yml
kubectl get pods -n bhagwat
kubectl get svc -n bhagwat
```

**Validation:**
- 2 pods running (`bms-app-*`)
- Service exposed via AWS LoadBalancer on port `80:31013/TCP`
- Application accessible at the external LoadBalancer DNS

---

### 6. Monitoring & Observability

**Deployed in:** `monitoring` namespace on the EKS cluster using Helm

**Stack:** `kube-prometheus-stack` (Prometheus + Grafana + Node Exporter + Alertmanager)

**Setup steps:**
```bash
# Install Helm
curl -fsSL https://raw.githubusercontent.com/helm/helm/main/scripts/get-helm-3 | bash

# Add Prometheus community repo
helm repo add prometheus-community https://prometheus-community.github.io/helm-charts
helm repo update

# Create namespace and install
kubectl create namespace monitoring
helm install prometheus prometheus-community/kube-prometheus-stack --namespace monitoring

# Expose as LoadBalancer
kubectl patch svc prometheus-grafana -n monitoring -p '{"spec": {"type": "LoadBalancer"}}'
kubectl patch svc prometheus-kube-prometheus-prometheus -n monitoring -p '{"spec": {"type": "LoadBalancer"}}'
```

**Access:**
| Service | URL |
|---|---|
| Grafana | `http://<LB-IP>:80` |
| Prometheus | `http://<LB-IP>:9090` |

**Grafana Dashboards imported:**
- **ID 15661** — Kubernetes Cluster Monitoring (K8S Dashboard)
- **Node Exporter Full** — System metrics (CPU, Memory, Disk, Network)

**Prometheus data source:** Configured and verified with "Successfully queried the Prometheus API."

**Slack Alerting:**

Grafana alerts are routed to a Slack channel (`#wipro-capstone-project`) via incoming webhook.

Alert rules created:
| Alert Name | Condition | Status |
|---|---|---|
| High CPU Usage | CPU > 75% for 5m | 🔴 Firing |
| High Memory Usage Alert | Memory threshold | ✅ Normal |
| Low Disk Space Alert | Disk threshold | 🔴 Firing |
| Pod Not Running | Pod count check | ✅ Normal |
| Pod Restarting Frequently | Restart rate check | ✅ Normal |

---

## Key Deliverables

| Deliverable | Link / Detail |
|---|---|
| Forked Repository | https://github.com/Bhagwat-io/Book-My-Show.git |
| Pull Request | https://github.com/Bhagwat-io/Book-My-Show/pull/1 |
| DockerHub Image | https://hub.docker.com/repository/docker/bhagwat07/bms-app/tags |
| Jira Board | Book My Show Project (BMSP) — all 6 tasks Done |
| SonarQube | Quality Gate: Passed |
| App on Docker | `http://<EC2-IP>:3000` |
| App on EKS | Accessible via LoadBalancer DNS |
| Grafana Dashboards | K8S metrics + Node Exporter |
| Slack Alerts | `#wipro-capstone-project` channel |

---

## Getting Started

### Prerequisites
- AWS account with EC2 and EKS access
- IAM user with programmatic access keys
- DockerHub account
- GitHub account
- Gmail account (for Jenkins email notifications)

### Quick Start

```bash
# 1. Clone the repository
git clone https://github.com/Bhagwat-io/Book-My-Show.git
cd Book-My-Show

# 2. Run with Docker locally
docker pull bhagwat07/bms-app:v1.0.0
docker run -d -p 3000:3000 bhagwat07/bms-app:v1.0.0

# 3. Access the app
open http://localhost:3000
```

### Deploy to Kubernetes
```bash
kubectl create ns bhagwat
kubectl apply -f deployment.yml
kubectl apply -f service.yml
kubectl get all -n bhagwat
```

---

## 📁 Repository Structure

```
Book-My-Show/
├── bookmyshow-app/        # React application source
├── Jenkinsfile            # Jenkins declarative pipeline
├── Jenkinsfile2           # Docker-only pipeline variant
├── Dockerfile             # Container build instructions
├── deployment.yml         # Kubernetes Deployment manifest
├── service.yml            # Kubernetes Service manifest
├── BMS-Document.txt       # Project notes
└── README.md              # This file
```

---

*DevOps Capstone Project — Bhagwat Kolekar*
