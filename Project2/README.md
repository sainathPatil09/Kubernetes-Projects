
# **Kubernetes Project Series (Project2)**
## **Multi-Tier Web Application Deployment on Kubernetes (Password Manager)**

A complete guide to deploying a three-tier application (Frontend + Backend + Database) on Kubernetes.

Complete Source Code: https://github.com/sainathPatil09/PasswordManager/tree/k8s

## **Understanding Kubernetes Concepts**

**Persistent Volumes (PV) and Persistent Volume Claims (PVC)**
1. **What are Persistent Volumes?**
    ```sh
    Persistent Volumes (PV) are cluster-level resources that represent physical storage in your cluster. Think of them as "storage units" that exist independently of any pod.

    - Lifecycle Independent: PVs exist beyond the lifecycle of individual pods
    - Cluster Resource: Managed by cluster administrators
    - Capacity Defined: Has specific size and access modes


    apiVersion: v1
    kind: PersistentVolume
    metadata:
    name: mongo-pv
    spec:
    capacity:
        storage: 5Gi
    accessModes:
        - ReadWriteOnce  # Can be mounted by a single node
    persistentVolumeReclaimPolicy: Retain
    storageClassName: manual
    hostPath:
        path: "/data/db"
    ```

2. **What are Persistent Volume Claims?**
    ```sh
    Persistent Volume Claims (PVC) are requests for storage by users. They are like "storage requisition forms" that pods use to claim a PV.

    - Namespace-scoped: Belongs to a specific namespace
    - Binding: Kubernetes automatically binds PVCs to suitable PVs
    - Pod Usage: Pods reference PVCs to access storage


    apiVersion: v1
    kind: PersistentVolumeClaim
    metadata:
    name: mongo-pvc
    spec:
    accessModes:
        - ReadWriteOnce
    resources:
        requests:
        storage: 5Gi
    storageClassName: manual
    ```

---

**Flow:**

- Admin creates PV (defines actual storage)
- User creates PVC (requests storage)
- Kubernetes binds PVC to matching PV
- Pod uses PVC to mount storage
- Data persists even if pod is deleted

---

**Access Modes**
| Access Mode      | Description                                   | Use Case                        |
|------------------|-----------------------------------------------|---------------------------------|
| ReadWriteOnce (RWO) | Volume mounted as read-write by a single node | Databases, single-instance apps |
| ReadOnlyMany (ROX)  | Volume mounted as read-only by many nodes     | Static content, shared configs  |
| ReadWriteMany (RWX) | Volume mounted as read-write by many nodes    | Shared file systems             |

---

**Reclaim Policies**
| Policy   | Behavior                                         | Use Case                |
|----------|--------------------------------------------------|-------------------------|
| Retain   | PV kept after PVC deletion, manual cleanup       | Production data         |
| Delete   | PV and storage deleted with PVC                  | Development/testing     |
| Recycle  | Deprecated (basic scrubbing)                     | Not recommended         |

---

**ConfigMaps vs Secrets**

ConfigMaps: Non-sensitive configuration
```sh
apiVersion: v1
kind: ConfigMap
metadata:
  name: backend-config
data:
  API_URL: "http://backend-service:3000"
  NODE_ENV: "production"
```

Secrets: Sensitive data (base64 encoded)
```sh
apiVersion: v1
kind: Secret
metadata:
  name: db-secret
type: Opaque
data:
  DB_PASSWORD: cGFzc3dvcmQxMjM=  # base64 encoded
```
