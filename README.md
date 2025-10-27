# 🐾 Spring PetClinic — Helm Deployment

This repository contains a **Helm chart** for deploying the **Spring PetClinic** application along with a **MySQL database** on a Kubernetes cluster.

The setup provides:
- Automated Kubernetes resource creation (Deployment, Service, ConfigMap, Secret)
- Seamless MySQL–Spring Boot integration
- Configurable environment using Helm values
- `initContainer` to ensure MySQL is ready before app startup
- Support for multiple environments via values override files

---

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

## ⚙️ Prerequisites

| Tool | Description | Verification Command |
|------|-------------|---------------------|
| **Kubernetes Cluster** | v1.19+ | `kubectl version --short` |
| **kubectl** | Kubernetes CLI | `kubectl version --client` |
| **Helm** | v3.8+ | `helm version --short` |
| **PersistentVolume Provisioner** | For MySQL storage | `kubectl get storageclass` |

## 🚀 Quick Start

### 1. Clone the Repository
```bash
git clone <repository-url>
cd helm-petclinic
```

### 2. Deploy to Kubernetes
```bash
# Deploy with default values
helm install petclinic .

# Deploy with custom values file
helm install petclinic . -f conf_values.yaml

# Deploy to specific namespace
helm install petclinic . -n petclinic-ns --create-namespace
```

### 3. Verify Deployment
```bash
# Check all resources
kubectl get all -l release=petclinic

# Check application status
helm status petclinic

# View application logs
kubectl logs -l app=spring-petclinic
```

### 4. Access the Application
```bash
# Get the application URL
kubectl get svc spring-petclinic-service -o jsonpath='{.status.loadBalancer.ingress[0].hostname}'

# Port forward for local access
kubectl port-forward svc/spring-petclinic-service 8080:80
```

## 🔧 Configuration

### Default Values (values.yaml)
```yaml
# MySQL Configuration
mysql:
  enabled: true
  image: "mysql:8.0"
  database: "petclinic"
  rootPassword: "petclinic"
  storage: "1Gi"
  service:
    port: 3306

# Spring PetClinic Configuration  
springApp:
  replicaCount: 1
  image: "springcommunity/spring-petclinic:latest"
  service:
    type: LoadBalancer
    port: 80
  livenessProbe:
    path: /actuator/health
  readinessProbe:
    path: /actuator/health
```

### Custom Configuration
Create `custom-values.yaml`:
```yaml
springApp:
  replicaCount: 3
  image: "myregistry/petclinic:v2.0"
  service:
    type: NodePort
    
mysql:
  rootPassword: "securepassword123"
  storage: "5Gi"
```

Deploy with custom values:
```bash
helm install petclinic . -f custom-values.yaml
```

## 📊 Environment Variables

The following environment variables are automatically configured:

| Variable | Description | Default |
|----------|-------------|---------|
| `SPRING_DATASOURCE_URL` | MySQL JDBC URL | `jdbc:mysql://mysql-service:3306/petclinic` |
| `SPRING_DATASOURCE_USERNAME` | Database username | `root` |
| `SPRING_DATASOURCE_PASSWORD` | Database password | From secret |
| `SPRING_PROFILES_ACTIVE` | Spring profile | `mysql` |

## 🔄 Operations

### Upgrade Deployment
```bash
# Upgrade with new values
helm upgrade petclinic . --set springApp.replicaCount=3

# Upgrade with values file
helm upgrade petclinic . -f conf_values.yaml
```

### Rollback Deployment
```bash
# View release history
helm history petclinic

# Rollback to previous version
helm rollback petclinic

# Rollback to specific revision
helm rollback petclinic 2
```

### Scale Application
```bash
# Scale Spring application
kubectl scale deployment spring-petclinic --replicas=3

# Or via Helm upgrade
helm upgrade petclinic . --set springApp.replicaCount=3
```

## 🗑️ Cleanup

### Uninstall Release
```bash
# Uninstall the release (keeps PVC)
helm uninstall petclinic

# Uninstall and delete PVC
helm uninstall petclinic
kubectl delete pvc -l app=mysql
```

### Complete Cleanup
```bash
# Delete all resources including persistent volumes
helm uninstall petclinic
kubectl delete pvc -l app=mysql
kubectl delete secret -l release=petclinic
```

## 🐛 Troubleshooting

### Common Issues

**Application Not Starting:**
```bash
# Check pod status
kubectl get pods -l app=spring-petclinic

# View application logs
kubectl logs -l app=spring-petclinic

# Check init container logs
kubectl logs -l app=spring-petclinic -c wait-for-mysql
```

**MySQL Connection Issues:**
```bash
# Check MySQL pod status
kubectl get pods -l app=mysql

# Check MySQL logs
kubectl logs -l app=mysql

# Verify service discovery
kubectl get svc mysql-service
```

**Persistent Volume Issues:**
```bash
# Check PVC status
kubectl get pvc -l app=mysql

# Check PV status
kubectl get pv

# Check storage class
kubectl get storageclass
```

### Debug Commands
```bash
# Dry run installation
helm install petclinic . --dry-run --debug

# Check generated manifests
helm template petclinic .

# Validate chart
helm lint .
```

## 🔐 Security Notes

- 🔒 MySQL root password is stored as a Kubernetes Secret
- 🔒 Consider using external secret management for production
- 🔒 Enable TLS for MySQL connections in production
- 🔒 Use private image registries for production images

## 📈 Monitoring

### Health Endpoints
```bash
# Application health
curl http://<service-ip>:8080/actuator/health

# Readiness check
curl http://<service-ip>:8080/actuator/health/readiness

# Liveness check  
curl http://<service-ip>:8080/actuator/health/liveness
```

### Kubernetes Health Checks
```bash
# Check pod status
kubectl get pods -l release=petclinic

# Check service endpoints
kubectl get endpoints

# View events
kubectl get events --sort-by=.metadata.creationTimestamp
```

## 🤝 Contributing

1. Fork the repository
2. Create a feature branch
3. Make your changes
4. Test the chart: `helm lint . && helm template test .`
5. Submit a pull request

## 📄 License

This Helm chart is licensed under the MIT License.

## 📞 Support

For support:
1. Check the [troubleshooting guide](#-troubleshooting)
2. Review Kubernetes logs
3. Open an issue in the repository

---

**Happy deploying! 🎉**
```
