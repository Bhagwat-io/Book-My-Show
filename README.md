🎬 BookMyShow Application – DevOps CI/CD Deployment
📌 Project Overview

This project demonstrates the implementation of a complete DevOps CI/CD pipeline for deploying the BookMyShow application.

The goal of this project was to automate the build, test, and deployment process using modern DevOps tools including GitHub, Jenkins, Docker, Kubernetes, and AWS.

The application is automatically built, containerized, deployed to Kubernetes, and monitored using Prometheus and Grafana.

🏗️ Architecture

Developer 👨‍💻
⬇
GitHub Repository 📦
⬇
Jenkins CI/CD Pipeline ⚙️
⬇
Docker Containerization 🐳
⬇
DockerHub Registry 📤
⬇
Kubernetes Deployment (AWS EKS) ☸️
⬇
Monitoring (Prometheus + Grafana) 📊

🛠️ Tools & Technologies

📋 Project Management --> 	Jira
🗂  Version Control	--> Git, GitHub
🔁 CI/CD	--> Jenkins
🔍 Code Quality	--> SonarQube
🐳 Containerization	--> Docker
📦 Container Registry	--> DockerHub
☸️ Container Orchestration	--> Kubernetes (AWS EKS)
☁️ Cloud Platform	--> AWS (EC2)
📊 Monitoring	--> Prometheus, Grafana
🔔 Alerts	--> Slack


⚙️ CI/CD Pipeline Stages

The Jenkins pipeline automates the following stages:

1️⃣ Clean Workspace
2️⃣ Checkout Source Code from GitHub
3️⃣ SonarQube Code Analysis 🔍
4️⃣ Install Dependencies 📦
5️⃣ Build Docker Image 🐳
6️⃣ Push Docker Image to DockerHub 📤
7️⃣ Deploy Application Container 🚀
8️⃣ Kubernetes Deployment on AWS EKS ☸️
9️⃣ Email Notification for Build Status 📧

🐳 Docker Implementation

Created a Dockerfile to containerize the BookMyShow application.

Built Docker images automatically through Jenkins pipeline.

Pushed images to DockerHub registry.

Verified container deployment on port 3000.

☸️ Kubernetes Deployment (AWS EKS)

The application was deployed on AWS Elastic Kubernetes Service (EKS).

Kubernetes Resources Used

📦 Deployment
🌐 Service (LoadBalancer)
📁 Namespace

Verification Commands
kubectl get pods
kubectl get svc
kubectl get nodes

The application was successfully exposed using a LoadBalancer service.

📊 Monitoring & Observability

Monitoring was implemented using Prometheus and Grafana.

Implemented Features

📈 Kubernetes cluster monitoring
🖥 Node exporter system metrics
📊 Grafana dashboards for visualization
⚠️ Alert rules for high CPU usage
🔔 Slack notifications for alerts

🔗 Project Links

GitHub Repository
https://github.com/Bhagwat-io/Book-My-Show

DockerHub Image
https://hub.docker.com/repository/docker/bhagwat07/bms-app

🎯 Key Learning Outcomes

✔️ Implemented end-to-end DevOps CI/CD pipeline
✔️ Deployed containerized applications with Docker
✔️ Managed Kubernetes deployments on AWS EKS
✔️ Implemented monitoring & alerting using Prometheus and Grafana
✔️ Integrated multiple DevOps tools into a single automated workflow

