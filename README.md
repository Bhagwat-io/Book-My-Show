BookMyShow Application – DevOps CI/CD Deployment
Project Overview

This project demonstrates the implementation of a complete DevOps CI/CD pipeline for deploying the BookMyShow application. The goal was to automate the software delivery process using modern DevOps tools and practices including CI/CD automation, containerization, Kubernetes orchestration, and monitoring.

The application is built and deployed through an automated pipeline using GitHub, Jenkins, Docker, and Kubernetes (AWS EKS) with monitoring implemented using Prometheus and Grafana.

Architecture

The project follows a typical DevOps workflow:

Developer → GitHub → Jenkins CI/CD → Docker → DockerHub → Kubernetes (EKS) → Monitoring (Prometheus & Grafana)

Tools & Technologies
Category	Tools
Project Management	Jira
Version Control	Git, GitHub
CI/CD	Jenkins
Code Quality	SonarQube
Containerization	Docker
Container Registry	DockerHub
Orchestration	Kubernetes (AWS EKS)
Cloud	AWS EC2
Monitoring	Prometheus, Grafana
Alerts	Slack
CI/CD Pipeline Stages

The Jenkins pipeline automates the following stages:

Clean Workspace

Checkout Source Code from GitHub

SonarQube Code Analysis

Install Application Dependencies

Build Docker Image

Push Docker Image to DockerHub

Deploy Application Container

Kubernetes Deployment on AWS EKS

Email Notification on Build Status

Docker Implementation

Created a Dockerfile to containerize the BookMyShow application.

Built Docker images automatically through Jenkins.

Pushed images to DockerHub registry.

Verified container deployment on port 3000.

Kubernetes Deployment (AWS EKS)

The application was deployed to AWS Elastic Kubernetes Service (EKS) using custom Kubernetes manifests.

Kubernetes Resources

Deployment

Service (LoadBalancer)

Namespace

Verification Commands
kubectl get pods
kubectl get svc
kubectl get nodes

The application was successfully exposed through a LoadBalancer service.

Monitoring & Observability

Monitoring was implemented using Prometheus and Grafana.

Features Implemented

Kubernetes cluster monitoring

Node exporter metrics

Jenkins performance monitoring

Grafana dashboards for visualization

Alert rules for high CPU usage

Slack notification integration for alerts

Repository

GitHub Repository:
https://github.com/Bhagwat-io/Book-My-Show

DockerHub Image:
https://hub.docker.com/repository/docker/bhagwat07/bms-app

Key Learning Outcomes

Implemented end-to-end DevOps CI/CD automation

Built and deployed containerized applications

Managed Kubernetes deployments on AWS EKS

Implemented monitoring and alerting for production environments

Integrated DevOps tools into a complete pipeline

Author

Bhagwat Kolekar
DevOps Trainee
