# CI/CD Pipeline for Node.js Application using Jenkins, Docker, Kubernetes and HPA

## Project Overview

This project demonstrates a complete CI/CD pipeline for a Node.js/React application using:

* GitHub
* Jenkins
* Docker
* Docker Hub
* Kubernetes (Minikube)
* Horizontal Pod Autoscaler (HPA)

The pipeline automatically builds, packages, publishes, and deploys the application whenever code changes are pushed to GitHub.

---

## Architecture

GitHub → Jenkins → Docker Build → Docker Hub → Kubernetes (Minikube) → HPA

---

## Prerequisites

Install the following components:

* Java 21
* Jenkins
* Git
* Docker
* Kubectl
* Minikube

Verify installation:

```bash
java -version
docker --version
kubectl version --client
minikube version
jenkins --version
```

---

## Project Structure

```text
project/
│
├── src/
├── public/
├── Dockerfile
├── Jenkinsfile
├── package.json
│
└── k8s/
    ├── deployment.yaml
    ├── service.yaml
    └── hpa.yaml
```

---

## Dockerfile

```dockerfile
FROM node:18-alpine

WORKDIR /app

COPY package*.json ./

RUN npm install

COPY . .

EXPOSE 3000

CMD ["npm","start"]
```

---

## Build Docker Image

```bash
docker build -t node-app:test .
```

Run locally:

```bash
docker run -p 3000:3000 node-app:test
```

---

## Docker Hub Login

```bash
docker login
```

Push image:

```bash
docker tag node-app:test <dockerhub-username>/node-app:latest

docker push <dockerhub-username>/node-app:latest
```

---

## Kubernetes Deployment

Apply manifests:

```bash
kubectl apply -f k8s/deployment.yaml
kubectl apply -f k8s/service.yaml
kubectl apply -f k8s/hpa.yaml
```

Check resources:

```bash
kubectl get pods
kubectl get svc
kubectl get hpa
```

---

## Jenkins Pipeline Stages

1. Checkout source code from GitHub
2. Build Docker image
3. Authenticate with Docker Hub
4. Push Docker image
5. Deploy to Kubernetes
6. Verify deployment

Example stages:

```text
Checkout
Build Docker Image
Docker Login
Push Docker Image
Deploy to Kubernetes
Verify Deployment
```

---

## Horizontal Pod Autoscaler

Enable metrics server:

```bash
minikube addons enable metrics-server
```

Check HPA:

```bash
kubectl get hpa
```

HPA automatically scales application pods based on CPU utilization.

---

## Verification

```bash
kubectl get pods
kubectl get deployments
kubectl get services
kubectl get hpa
```

Access application:

```bash
minikube service <service-name>
```

---

## CI/CD Workflow

1. Developer pushes code to GitHub.
2. Jenkins pipeline starts automatically.
3. Docker image is built.
4. Image is pushed to Docker Hub.
5. Kubernetes deployment is updated.
6. Application pods are recreated with the latest image.
7. HPA monitors CPU usage and scales pods automatically.

---

## Expected Outcome

A fully automated CI/CD pipeline that:

* Builds application automatically
* Creates Docker images
* Pushes images to Docker Hub
* Deploys to Kubernetes
* Supports automatic horizontal scaling through HPA
