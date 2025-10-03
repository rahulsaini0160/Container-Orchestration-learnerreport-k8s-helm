# MERN Application — Kubernetes, Helm & Jenkins CI/CD Deployment

This project demonstrates a complete **containerized deployment pipeline** for a **MERN stack** (MongoDB, Express.js, React.js, Node.js) application using **Kubernetes**, **Helm**, and **Jenkins**.

It includes:
-  Kubernetes manifests for frontend, backend, and optional MongoDB.
-  A production-ready Helm chart for templated deployments.
-  A Jenkins pipeline (Groovy) for CI/CD automation — build, push, deploy.
-  Documentation outlining setup, assumptions, and troubleshooting.

---

## Repositories

- **Frontend**: [learnerReportCS_frontend](https://github.com/UnpredictablePrashant/learnerReportCS_frontend)  
- **Backend**: [learnerReportCS_backend](https://github.com/UnpredictablePrashant/learnerReportCS_backend)

---

## Project Structure


---

##  Prerequisites

- Docker & Kubernetes (Minikube, EKS, GKE, AKS, etc.)
- Helm 3+
- Jenkins (with Docker, Kubernetes, and Git plugins)
- Container Registry (e.g., GitHub Container Registry `ghcr.io` or Docker Hub)

---

## Step-by-Step Setup

## Build & Push Images

```bash
# Backend
docker build -t ghcr.io/<org>/learnerreport-backend:latest -f learnerReportCS_backend/Dockerfile learnerReportCS_backend
docker push ghcr.io/<org>/learnerreport-backend:latest

# Frontend
docker build -t ghcr.io/<org>/learnerreport-frontend:latest -f learnerReportCS_frontend/Dockerfile learnerReportCS_frontend
docker push ghcr.io/<org>/learnerreport-frontend:latest



├── k8s/
│ ├── mongodb-deployment.yml
├── helm/
│ └── learnerreportcs/
│ ├── Chart.yaml
│ ├── values.yaml
│ └── templates/
│ ├── deployment-frontend.yaml
│ ├── service-frontend.yaml
│ ├── deployment-backend.yaml
│ ├── service-backend.yaml
├── Jenkinsfile
└── README.md
