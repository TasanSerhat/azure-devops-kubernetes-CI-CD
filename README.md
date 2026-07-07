# Azure DevOps CI/CD Pipeline with Local Kubernetes

## 🚀 Project Overview

This project demonstrates a complete **End-to-End CI/CD Pipeline** connecting **Azure DevOps** with a local **Kubernetes** cluster.

The objective is to automate the deployment lifecycle of a containerized web application. Unlike standard cloud-to-cloud pipelines, this project implements a **Self-Hosted Agent** architecture to bridge the gap between Azure Cloud and a local development environment (Docker Desktop), simulating a hybrid-cloud scenario.

---

## 📋 Prerequisites

To run or replicate this project, the following tools and configurations are required:

- **Azure DevOps Account**: For repository hosting and pipeline management.
- **Docker Desktop**: With Kubernetes enabled (for the local cluster).
- **VS Code & Git**: For development and version control.
- **kubectl**: Command-line tool for interacting with the Kubernetes cluster.
- **Azure Pipelines Self-hosted Agent**: Configured locally to execute build and deploy tasks.

---

## 📂 Repository Structure

The project files are organized as follows:

- `azure-pipelines.yaml`: The CI/CD configuration file defining triggers, the agent pool, and execution steps (Build & Deploy).
- `Dockerfile`: Instructions to build the lightweight Docker image for the web application.
- `deployment.yaml`: Kubernetes manifest file defining the **Deployment** (ReplicaSet) and **Service** (NodePort) configuration.
- `index.html`: The source code of the web application.
- `README.md`: Technical documentation of the project.

---

## 🏗 Architectural Workflow

The pipeline automation follows these steps:

1. **Code Commit**: Code changes are committed and pushed to the Azure DevOps Repository (or GitHub mirror) via VS Code.
2. **Trigger**: The push action automatically triggers the Azure Pipeline execution.
3. **Build**: The **Self-Hosted Agent** running on the local machine picks up the job and executes `docker build` to create the application image.
4. **Deploy**: The agent executes `kubectl apply -f deployment.yaml` to deploy the fresh image to the local Kubernetes cluster immediately.

---

## ⚙️ Configuration & Setup

### 1. The Self-Hosted Agent Strategy

This project is configured to use a specific agent pool named `MyLocalPool` in the `azure-pipelines.yaml` file:
```yaml
pool:
  name: MyLocalPool
```

**Note for Cloners**: Since this pipeline targets a local Kubernetes cluster behind a firewall (localhost), it requires a Self-Hosted Agent. If you fork this project, you must create your own Self-Hosted Agent in Azure DevOps and update the `pool: name` value in the YAML file.

### 2. Security & Secrets

- **No Hardcoded Secrets**: This repository does not contain Personal Access Tokens (PAT) or sensitive credentials.
- **Authentication**: The connection between the Agent and Azure DevOps is secured via PAT configured during the agent installation phase, ensuring the codebase remains clean and secure.

---

## ✅ Verification & Results

### Successful Pipeline Execution

The pipeline runs successfully upon every push to the repository, automating the Build and Deploy stages.

### Kubernetes Deployment Status

The application is successfully deployed to the Kubernetes cluster and pods are in `Running` state.

**Command**: `kubectl get pods`

**Output**:
```bash
NAME                                          READY   STATUS    RESTARTS      AGE
devops-project-deployment-fc7d75c8f-75wxm     1/1     Running   1 (12m ago)   19m
```

### Accessing the Application

The application is exposed via a NodePort service and is accessible locally at:

**URL**: `http://localhost:30005`

---

## 👨‍💻 Credits

This project serves as a demonstration of DevOps practices using Azure Pipelines and Kubernetes.
