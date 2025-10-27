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


---

## ⚙️ Prerequisites

Ensure the following tools are installed and configured:

| Tool | Description | Verify |
|------|--------------|--------|
| **kubectl** | Kubernetes CLI | `kubectl version --client` |
| **helm** | Helm 3+ CLI | `helm version` |
| **aws-cli** *(if using EKS)* | AWS CLI to manage EKS clusters | `aws --version` |
| **Cluster Access** | Ensure kubeconfig is set | `kubectl get nodes` |

---

## ☁️ Connect to Cluster

For EKS:
```bash
aws eks update-kubeconfig --region us-west-2 --name eesha
kubectl get nodes
