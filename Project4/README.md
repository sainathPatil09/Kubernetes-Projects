# **Kubernetes Project Series: Project4**

## **CI/CD on Kubernetes with Helm Charts**

### **Project overview**

This repository demonstrates a concise CI/CD workflow for deploying containerized applications to Kubernetes using Helm charts. It includes example Helm charts, deployment manifests, and pipeline configuration to automate build, and release processes.

**Note:** We are not using ArgoCD for CD(continous deployment) rather we are using pipeline flow with helm charts.

**Key points:**
- **Purpose:** Automate build → image → deploy using Helm-driven releases and rollbacks.
- **Components:** Docker image builds, Helm charts, Kubernetes manifests, and CI pipeline templates.
- **Goals:** reproducible deployments, zero-downtime updates, and easy rollbacks.



<!-- scp -i .\ec2_keypair.pem  -r D:\work\Kubernetes-Projects\Project3\k8s\ ubuntu@13.233.212.232:/home/ubuntu/project4 -->