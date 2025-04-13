

## **Project Overview**  
- 🚀 Deployment of a containerized Node.js calculator application to Kubernetes  
- ⚙️ Configuration of Kubernetes manifests (Deployment + Service)  
- 🔄 CI/CD-ready structure with Docker and Kubernetes integration  

## **Prerequisites**  
| Tool | Installation Guide |  
|------|-------------------|  
| Minikube | [Installation Docs](https://minikube.sigs.k8s.io/docs/start/) |  
| kubectl | [Setup Guide](https://kubernetes.io/docs/tasks/tools/) |  
| Docker | [Get Docker](https://docs.docker.com/get-docker/) |  
| Node.js | [Download](https://nodejs.org/en/download/) |  

## **Quick Deployment Guide**  

### **1. Initialize Minikube Cluster**  
```bash  
minikube start --memory=4096 --cpus=2 --driver=docker  
minikube status  # Verify cluster status  
```

### **2. Build and Deploy**  
```bash  
# Build Docker image in Minikube environment  
eval $(minikube docker-env)  
docker build -t calculator-app:1.0 .  

# Apply Kubernetes configurations  
kubectl apply -f deployment.yaml  
kubectl apply -f service.yaml  
```

### **3. Access the Application**  


**Minikube Service URL**  
```bash  
minikube service calculator-service --url  
```  

## **Repository Structure**  
```  
.  
├                # Kubernetes manifests  
├── deployment.yaml      # ReplicaSet configuration  
├── service.yaml         # Service exposure specs  
├── Dockerfile               # Container build instructions  
├── .gitignore               # Ignore node_modules, etc.  
└── README.md                # This documentation  
```  

## **Key Configuration Files**  

### **Deployment Manifest**  
```yaml  
# deployment.yaml  
apiVersion: apps/v1  
kind: Deployment  
metadata:  
  name: calculator-deployment  
spec:  
  replicas: 2  
  selector:  
    matchLabels:  
      app: calculator  
  template:  
    metadata:  
      labels:  
        app: calculator  
    spec:  
      containers:  
      - name: calculator  
        image: calculator-app:1.0  
        imagePullPolicy: Never  
        ports:  
        - containerPort: 3000  
```  

### **Service Manifest**  
```yaml  
# service.yaml  
apiVersion: v1  
kind: Service  
metadata:  
  name: calculator-service  
spec:  
  selector:  
    app: calculator  
  ports:  
    - protocol: TCP  
      port: 80  
      targetPort: 3000  
  type: NodePort  
```  

## **Troubleshooting Guide**

| Error | Solution |  
|-------|----------|  
| `ImagePullBackOff` | Ensure `imagePullPolicy: Never` for local images |  
| `Pending` pods | Increase resources: `minikube start --memory=4096 --cpus=4` |  
| Port conflicts | Verify no local services using port 8080 |  

---  
