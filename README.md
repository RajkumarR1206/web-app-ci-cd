---

# **Flask CI/CD Pipeline with Docker, GitHub Actions, and Kubernetes**

This project demonstrates a complete **end-to-end CI/CD pipeline** for a containerized Flask web application.
The solution includes:

* Automated builds and tests using **GitHub Actions**
* Containerization using **Docker**
* Image distribution via **Docker Hub**
* Kubernetes deployment using **kubectl**
* Zero-downtime updates using rolling deployments
* Local Kubernetes cluster using **Minikube / K8s on Ubuntu**

This project replicates real-world DevOps workflows used in production environments.

---

## 🚀 **Architecture Overview**

```
Developer Pushes Code → GitHub Actions CI → Docker Build → Docker Hub
                                            ↓
                                       CD Deployment
                                            ↓
                                      Kubernetes Cluster
```

---

## 🧰 **Tech Stack**

| Component           | Technology Used       |
| ------------------- | --------------------- |
| Backend Application | Python Flask          |
| Version Control     | Git + GitHub          |
| CI Pipeline         | GitHub Actions        |
| Containerization    | Docker                |
| Image Registry      | Docker Hub            |
| Deployment          | Kubernetes (kubectl)  |
| Local Cluster       | Minikube / Ubuntu K8s |
| OS                  | WSL2/Ubuntu           |

---

## 📦 **Project Structure**

```bash
flask-ci-cd/
│
├── app.py               # Flask application
├── requirements.txt     # Python dependencies
├── Dockerfile           # App containerization
├── k8s/
│   ├── deployment.yaml  # Kubernetes Deployment
│   └── service.yaml     # Kubernetes Service (NodePort)
│
└── .github/workflows/
    ├── ci.yml          # CI Pipeline – Build & Push Docker Image
    └── cd.yml          # CD Pipeline – Deploy to Kubernetes
```

---

##  **How the CI/CD Pipeline Works**

### **1. CI — Build & Push Docker Image**

GitHub Actions automatically:

* Pulls latest code
* Installs dependencies
* Builds Docker image
* Tags image as `latest`
* Pushes image to Docker Hub

### **2. CD — Deploy to Kubernetes**

After pushing the image:

* GitHub Actions connect to the Kubernetes cluster
* Applies updated deployment file
* Triggers a rolling restart
* Pulls new image version automatically

---

##  **Docker Instructions**

### Build image manually (optional)

```bash
docker build -t flask-ci-cd-app .
```

### Tag for Docker Hub

```bash
docker tag flask-ci-cd-app:latest <dockerhub-username>/flask-ci-cd-app:latest
```

### Push to Docker Hub

```bash
docker push <dockerhub-username>/flask-ci-cd-app:latest
```

---

## ☸️ **Kubernetes Deployment**

### Apply Deployment

```bash
kubectl apply -f k8s/deployment.yaml
```

### Apply Service

```bash
kubectl apply -f k8s/service.yaml
```

### Restart Deployment

```bash
kubectl rollout restart deployment flask-app
```

### Check Pods

```bash
kubectl get pods
```

### Check Service

```bash
kubectl get svc flask-service
```

---

## **Accessing the Application**

### Option 1: Using `kubectl port-forward`

Works reliably on all systems:

```bash
kubectl port-forward deployment/flask-app 5000:5000
```

Visit in browser:
👉 **[http://localhost:5000/](http://localhost:5000/)**

### Option 2: Using NodePort (if Minikube supports it)

```bash
minikube ip
```

Access at:

```
http://<minikube-ip>:30001
```

---

##  **Kubernetes Files**

### **Deployment**

* Deploys Flask app
* Pulls image from Docker Hub
* Always pulls latest image

### **Service**

* Exposes application using NodePort
* Port: `30001`

---

##  **GitHub Actions Workflow (CI/CD)**

The pipeline:

* Runs on every `push` to `main`
* Logs in to Docker Hub
* Builds & pushes the image
* Applies Kubernetes deployment
* Performs rolling update

The workflow file is located at:

```
.github/workflows/ci-cd.yml
```

---

##  **Testing the Pipeline**

To test the CI/CD end-to-end:

1. Change anything in `app.py`

2. Commit & push

3. GitHub Actions will :

   * Build the Docker image
   * Push to Docker Hub
   * Restart Kubernetes deployment

4. Run:

```bash
kubectl get pods
```

You should see a new pod with the updated version.

---

## **Future Enhancements**

You can extend this project with:

* Helm charts
* Ingress + TLS
* Prometheus + Grafana monitoring
* HashiCorp Vault for secrets
* Terraform-based cluster provisioning
* Canary deployments / Blue-Green

---
