# RabbitMQ Cluster Deployment on Kubernetes using Helm

## To Access the Application onyour Local you must have minikube installed on your Machine
### Clone The Repo
### Start minikube 
### Install helm cluster
```
helm install rabbitmq-cluster .
```
### Run the Cmd 
```
minikube service rabbitmq-management --url 
```
## OR
```
minikube service rabbitmq-management
```
## Overview
This project deploys RabbitMQ in a **3-node clustered configuration** using **Kubernetes StatefulSets**. The RabbitMQ nodes automatically discover each other, ensuring high availability and persistent storage using PVCs. The deployment is managed using **Helm** for easy customization and scalability.

## Cluster Setup
- Deploys RabbitMQ as a **StatefulSet** with **3 replicas**.
- Uses **Persistent Volume Claims (PVCs)** for persistent storage.
- Ensures automatic **cluster formation and state synchronization**.
- Provides **high availability** with failure recovery mechanisms.

## Helm Chart Structure
The project is structured as follows:
```
rabbitmq-cluster/
├── templates/                   
│   ├── statefulset.yaml         
│   ├── service.yaml             
│   ├── management-service.yaml  
├── Chart.yaml                   
├── values.yaml                  
     
```

## Production-Ready Configuration
### **StatefulSet**
- Ensures stable pod identities (`rabbitmq-0`, `rabbitmq-1`, `rabbitmq-2`).
- Uses **PVCs** to persist RabbitMQ data across restarts.

### **Persistence**
- PVC ensures messages survive pod restarts.
- Storage class can be customized in `values.yaml`.

### **Replicas**
- Default: **3 replicas** (Can be changed in `values.yaml`).
- Pods automatically form a RabbitMQ cluster.

### **Health Checks**
- **Readiness Probe**: Ensures RabbitMQ is ready to accept traffic.
- **Liveness Probe**: Restarts unhealthy nodes.

## Services
- **Internal Service**: Handles inter-node communication.
- **Management UI**: Exposes RabbitMQ web UI (`http://<minikube-ip>:<port>`).

## Resource Requests & Limits
- CPU & Memory limits are set in `values.yaml`.
- Ensures stable performance and avoids resource exhaustion.


## Scalability & Customization
- **Scale cluster** by updating `replicaCount` in `values.yaml`.
- Configurable **storage, resources, user credentials, and clustering settings**.

## Deployment Steps
1. **Install Helm (if not already installed)**
   ```
   curl https://raw.githubusercontent.com/helm/helm/main/scripts/get-helm-3 | bash
   ```
2. **Deploy RabbitMQ using Helm**
   ```
   helm install rabbitmq-cluster .
   ```
3. **Verify deployment**
   ```
   kubectl get pods -l app=rabbitmq
   ```
4. **Access RabbitMQ UI**
   ```
   minikube service rabbitmq-management --url
   ```

## Scaling RabbitMQ Cluster
- Update `values.yaml`:
  ```
  replicaCount: 5  # Scale up to 5 replicas
  ```
- Apply changes:
  ```
  helm upgrade rabbitmq-cluster .
  ```

## Troubleshooting
| As a mention this application deployed on local machine using minikube cluster it is not able to access with default port because it doesnot expose nodeport service to machone directly Use `minikube service rabbitmq-management --url` |
| Persistent storage issues | Check PVC status with `kubectl get pvc`. |
| oR we can use `Loadbalancer instead of Nodeport`|
| Also Access with `port forwarding`|

