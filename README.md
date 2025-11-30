---

## Python Flask CI/CD Pipeline with Docker, GitHub Actions, and Kubernetes

This project demonstrates a complete **end-to-end CI/CD pipeline** for a containerized Flask web application.
The solution includes:-

* Automated builds and tests using **GitHub Actions**
* Containerization using **Docker**
* Image distribution via **Docker Hub**
* Kubernetes deployment using **kubectl**
* Rolling deployments (zero downtime)
* Local Kubernetes cluster using **Minikube / Ubuntu K8s**

This project replicates real-world DevOps workflows used in production.

---

##  **Architecture Overview**

```
Developer Pushes Code 
        ↓
GitHub Actions CI → Docker Build → Docker Hub
        ↓
GitHub Actions CD → Kubernetes Deployment
        ↓
Updated Application Running in Cluster
```

---

## **Sample Output:**


Github Actions workflow:

<img width="1363" height="517" alt="github-action-flow" src="https://github.com/user-attachments/assets/b4ad71ba-1822-4e2e-880b-d24b2b854e48" />

CI workflow:
<img width="1347" height="599" alt="ci-flow" src="https://github.com/user-attachments/assets/c1d841bc-813f-424e-9ba9-a96767c87c96" />

CD workflow:
<img width="1353" height="561" alt="CD-flow" src="https://github.com/user-attachments/assets/3a457626-4006-4c48-9918-4af3442533f2" />

Pushed into Docker hub:
<img width="1298" height="582" alt="docker-hub" src="https://github.com/user-attachments/assets/bce645dd-baf7-4bd7-8876-1b7402a723c3" />

Running using Self hosted runner: 
<img width="921" height="219" alt="action-self-hosted-runner" src="https://github.com/user-attachments/assets/04c4f2f0-d693-4366-80a8-b80da7467cc2" />

Pod details: 
<img width="935" height="199" alt="pod-detail" src="https://github.com/user-attachments/assets/06996a5e-7c56-4b4f-acbe-547ba87bc915" />

Flask app Result:

<img width="844" height="293" alt="Flask-app-result" src="https://github.com/user-attachments/assets/b815173a-d58b-439c-a1ef-aa43d7d632e1" />



---

##  **Tech Stack**

| Component        | Technology               |
| ---------------- | ------------------------ |
| Backend          | Python Flask             |
| Version Control  | Git + GitHub             |
| CI Pipeline      | GitHub Actions           |
| Containerization | Docker                   |
| Image Registry   | Docker Hub               |
| Deployment       | Kubernetes               |
| Local Cluster    | Minikube / Ubuntu / Kind |
| OS               | WSL2 + Ubuntu            |

---

##  **Project Structure**

```
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
    ├── ci.yml           # CI Pipeline – Build & Push Docker Image
    └── cd.yml           # CD Pipeline – Deploy to Kubernetes
```

---

##  **How the CI/CD Pipeline Works**

### **1. CI Pipeline — Build & Push Docker Image**

GitHub Actions automatically:

1. Pulls latest code
2. Installs dependencies
3. Builds Docker image
4. Tags image as `latest`
5. Pushes image to Docker Hub

---

### **2. CD Pipeline — Deploy to Kubernetes**

After the image is pushed:

1. GitHub Actions connects to the Kubernetes cluster
2. Applies Kubernetes manifests
3. Triggers a rolling update
4. New pods automatically pull the latest image

---

##  **Docker Instructions**

**Build image manually (optional)**

```bash
docker build -t flask-ci-cd-app .
```

**Tag for Docker Hub**

```bash
docker tag flask-ci-cd-app:latest <dockerhub-username>/flask-ci-cd-app:latest
```

**Push to Docker Hub**

```bash
docker push <dockerhub-username>/flask-ci-cd-app:latest
```

---

##  **Kubernetes Deployment**

**Apply Deployment**

```bash
kubectl apply -f k8s/deployment.yaml
```

**Apply Service**

```bash
kubectl apply -f k8s/service.yaml
```

**Restart Deployment**

```bash
kubectl rollout restart deployment flask-app
```

**Check Pods**

```bash
kubectl get pods
```

**Check Service**

```bash
kubectl get svc flask-service
```

---

##  **Accessing the Application**

### **Option 1: With kubectl port-forward (recommended)**

```bash
kubectl port-forward deployment/flask-app 5000:5000
```

Open browser:

 [http://localhost:5000/](http://localhost:5000/)

---

### **Option 2: Using NodePort**

If Minikube supports NodePort:

```bash
minikube ip
```

Access:

```
http://<minikube-ip>:30001
```

---

##  **Kubernetes Files Overview**

### **Deployment**

* Deploys Flask app
* Pulls latest Docker image
* ImagePullPolicy: Always

### **Service**

* Exposes application
* NodePort: **30001**

---

##  **GitHub Actions Workflows**

Your workflows:

* `.github/workflows/ci.yml` → CI pipeline
* `.github/workflows/cd.yml` → CD pipeline

They perform:

1. Build Docker image
2. Login to Docker Hub
3. Push image
4. Connect to cluster
5. Update Deployment
6. Trigger Rolling Update

---

##  **Testing the Pipeline**

Steps:

1. Edit `app.py`
2. Commit & push
3. GitHub Actions will:

   * Build image
   * Push to Docker Hub
   * Restart Deployment

Check new pod:

```bash
kubectl get pods
```

You should see a **new pod** running latest code.

---

##  **Future Enhancements**

You can extend this project with:

* Helm Charts
* NGINX Ingress + TLS
* Prometheus + Grafana monitoring
* HashiCorp Vault for secrets
* Terraform-managed Kubernetes cluster
* Canary / Blue-Green deployments

---

