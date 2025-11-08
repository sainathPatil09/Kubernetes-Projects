# **Kubernetes Project Series: Project4**

## **CI/CD on Kubernetes with Helm Charts**

### **Project overview**

This repository demonstrates a concise CI/CD workflow for deploying containerized applications to Kubernetes using Helm charts. It includes example Helm charts, deployment manifests, and pipeline configuration to automate build, and release processes.

**Note:** We are not using ArgoCD for CD(continous deployment) rather we are using pipeline flow with helm charts.

**Key points:**
- **Purpose:** Automate build → image → deploy using Helm-driven releases and rollbacks.
- **Components:** Docker image builds, Helm charts, Kubernetes manifests, and CI pipeline templates.
- **Goals:** reproducible deployments, zero-downtime updates, and easy rollbacks.

### **What is Helm**
- Helm is a package manager for Kubernetes
- Just like `apt` for linux, `npm` for node.js and `pip` for python
- Instead of manually creating and applying many Kubernetes YAML files, Helm lets you bundle them into a single reusable package called a **Helm Chart**.

### **Why to use Helm**

| Without Helm                    | With Helm                           |
| ------------------------------- | ----------------------------------- |
| Apply 10–20 YAML files manually | 1 command deploy                    |
| Hardcoded values                | Configurable via `values.yaml`      |
| No versioning                   | Maintain release history            |
| Hard to rollback                | Instant rollback                    |
| Must remember order             | Helm handles order by resource type |
| Repetitive work                 | Reusable chart                      |

- simply Helm is Faster, Repeatable, Easier to manage, Production friendly

### **Helm Commands**
### Install / Uninstall
| Command                                    | Purpose                  |
| ------------------------------------------ | ------------------------ |
| `helm install <release-name> <chart-path>` | Deploy a chart           |
| `helm install myapp ./mychart`             | Install from local chart |
| `helm install myapp bitnami/nginx`         | Install from remote repo |
| `helm uninstall <release-name>`            | Delete a release         |
| `helm uninstall myapp`                     | Remove deployed app      |

### Upgrade & Rollback
| Command                              | Purpose                      |
| ------------------------------------ | ---------------------------- |
| `helm upgrade <release> <chart>`     | Upgrade existing release     |
| `helm upgrade myapp ./mychart`       | Upgrade from local chart     |
| `helm rollback <release> <revision>` | Rollback to previous version |
| `helm rollback myapp 1`              | Rollback myapp to revision 1 |

### Release Information
| Command                  | Purpose                         |
| ------------------------ | ------------------------------- |
| `helm list`              | List all Helm releases          |
| `helm list -A`           | List releases in all namespaces |
| `helm status <release>`  | Show release status             |
| `helm history <release>` | Show release revision history   |

### Dry Run / Debug
| Command                                | Purpose                         |
| -------------------------------------- | ------------------------------- |
| `helm install myapp ./chart --dry-run` | Simulate install, no deployment |
| `helm install myapp ./chart --debug`   | Debug output                    |
| `helm template ./chart`                | Render YAML without deploying   |

### Create & Package Charts
| Command                     | Purpose                   |
| --------------------------- | ------------------------- |
| `helm create <chart-name>`  | Create a new Helm chart   |
| `helm create mychart`       | Example                   |
| `helm package <chart-path>` | Package chart into `.tgz` |
| `helm lint ./chart`         | Validate chart for errors |

### Comman Flags
| Flag                  | Meaning                        |
| --------------------- | ------------------------------ |
| `-n <namespace>`      | Deploy to specific namespace   |
| `--create-namespace`  | Create namespace if not exists |
| `--set key=value`     | Override values directly       |
| `-f values-prod.yaml` | Use custom values file         |



<!-- scp -i .\ec2_keypair.pem  -r D:\work\Kubernetes-Projects\Project3\k8s\ ubuntu@13.233.212.232:/home/ubuntu/project4 -->


| Order (applied first → last) | Resource Type                                                                    |
| ---------------------------- | -------------------------------------------------------------------------------- |
| 1                            | Namespaces                                                                       |
| 2                            | NetworkPolicy                                                                    |
| 3                            | ResourceQuota, LimitRange                                                        |
| 4                            | ServiceAccount, Secret, ConfigMap                                                |
| 5                            | PersistentVolume, PersistentVolumeClaim, StorageClass                            |
| 6                            | Deployment, StatefulSet, DaemonSet                                               |
| 7                            | Service                                                                          |
| 8                            | Ingress                                                                          |
| 9                            | CRDs (CustomResourceDefinitions) are always installed first if in `crds/` folder |
