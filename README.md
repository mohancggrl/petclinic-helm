# 🐾 Spring PetClinic — Helm Deployment

This repository contains a **Helm chart** for deploying the **Spring PetClinic** application along with a **MySQL database** on a Kubernetes cluster.

The setup provides:
- Automated Kubernetes resource creation (Deployment, Service, ConfigMap, Secret)
- Seamless MySQL–Spring Boot integration
- Configurable environment using Helm values
- Wait-for-MySQL initContainer for clean startup
- Support for multiple environments via values override files

---

## 📁 Repository Structure
# 🐾 Spring PetClinic - Helm Deployment

## 📁 Repository Structure
```bash
helm-petclinic/
├── Chart.yaml
├── values.yaml
├── conf_values.yaml
├── templates/
│   ├── mysql-secret.yaml
│   ├── mysql-deployment.yaml
│   ├── mysql-service.yaml
│   ├── spring-deployment.yaml
│   ├── spring-service.yaml
│   └── configmap.yaml
└── conf_files/
    └── application.properties



---

# 🚀 Helm Deployment Guide for Kubernetes

## 📋 Table of Contents
- [Prerequisites](#-prerequisites)
- [Cluster Connection](#-connect-to-cluster)
- [Helm Basics](#-helm-basics)
- [Deployment Examples](#-deployment-examples)
- [Verification](#-verification)
- [Troubleshooting](#-troubleshooting)

---

## ⚙️ Prerequisites

Ensure the following tools are installed and configured:

| Tool | Description | Verify Command |
|------|-------------|----------------|
| **kubectl** | Kubernetes CLI | `kubectl version --client` |
| **helm** | Helm 3+ CLI | `helm version` |
| **aws-cli** | AWS CLI for EKS | `aws --version` |
| **Cluster Access** | kubeconfig setup | `kubectl get nodes` |

### Installation Commands

**Install kubectl:**
```bash
# macOS
brew install kubectl

# Linux
curl -LO "https://dl.k8s.io/release/$(curl -L -s https://dl.k8s.io/release/stable.txt)/bin/linux/amd64/kubectl"
sudo install -o root -g root -m 0755 kubectl /usr/local/bin/kubectl

# Windows
choco install kubernetes-cli
