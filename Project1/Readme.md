# **React App on Kubernetes (Minikube on EC2)**

This project demonstrates how to deploy a simple React application on **Kubernetes** using **Minikube** inside an AWS EC2 instance.  
It covers key Kubernetes concepts: **Pods, Deployments, Services, Rolling Updates, and Port Forwarding**.

---

### **Installation and Setup**
- Launch AWS EC2 `(t2.medium)`, storage `30GB`
- allow ports `80` and `8080` in security group
- update ec2 `sudo apt update -y`
- **Install Docker**
    ```sh
        sudo apt install -y docker
        sudo usermod -aG docker $USER && newgrp docker        # add user to docker group
        docker ps                               #test
    ```
- **Install kubectl**
    ```sh
    curl -LO "https://dl.k8s.io/release/$(curl -L -s https://dl.k8s.io/release/stable.txt)/bin/linux/amd64/kubectl"

    curl -LO "https://dl.k8s.io/release/$(curl -L -s https://dl.k8s.io/release/stable.txt)/bin/linux/amd64/kubectl.sha256"

    echo "$(cat kubectl.sha256)  kubectl" | sha256sum --check       #   kubectl: OK

    chmod +x kubectl
    sudo mv kubectl /usr/local/bin/
    kubectl version --client
    ```
- **Install Minikube**
    ```sh
    curl -LO https://github.com/kubernetes/minikube/releases/latest/download/minikube-linux-amd64
    sudo install minikube-linux-amd64 /usr/local/bin/minikube && rm minikube-linux-amd64

    minikube start  

    kubectl get nodes    # verify
    ```

---

### **Concepts**
#### **1. Pod**
- The smallest deployable unit in Kubernetes.
- Wraps one or more containers (usually one container per pod).
- Pods are ephemeral → if a pod dies, it won’t come back automatically unless managed by something else (like a Deployment).
- Each pod has its own IP address inside the cluster.

#### **2. Deployment**
- A higher-level controller that manages Pods.
- Ensures the desired number of replicas (copies of your pod) are always running.
- Supports:
    - Scaling (increase/decrease replicas)
    - Rolling updates (update without downtime)
    - Rollback (return to older version if needed)
- `Example:` Instead of manually running 2 pods, you define a Deployment with `replicas: 2` — Kubernetes keeps them alive.


    #### **Different Workload Resources in Kubernetes**
    | Resource                  | Use Case                                |
    | ------------------------- | --------------------------------------- |
    | **Deployment**            | Stateless apps, rolling updates         |
    | **ReplicaSet**            | Keep pods running (used by Deployment)  |
    | **StatefulSet**           | Stateful apps, databases, ordered pods  |
    | **DaemonSet**             | Run pod on every node (logging/monitor) |
    | **Job**                   | One-time tasks, batch processing        |
    | **CronJob**               | Scheduled tasks                         |

#### **3. Service**
- Pods are temporary; their IPs change when recreated.
- A Service provides a stable network endpoint to access a set of Pods.
- It load-balances traffic across pods.
- Types of Services:
- ClusterIP (default) -> accessible only inside cluster.
- NodePort -> exposes service on `<NodeIP>:<Port>`.
- LoadBalancer -> cloud provider creates external LB.
- `Example:` Your Deployment runs 2 pods of React app -> Service balances traffic between them and gives a stable IP/port.

#### **4. Rolling Updates**
- Rolling updates are handled by Deployments in Kubernetes.
- They let you update your application version with zero downtime.

#### **5. Port Forwarding**
- Port forwarding lets you temporarily map a local port on your EC2 instance -> a port inside a Pod/Service.


### **Applying manifest**
- **Create Namespace**
    - `kubectl apply -f namespace.yml`
- **Deployment.yml**
    - `kubectl apply -f deployment.yml`
- **Service.yml**
    - `kubectl apply -f service.yml`

- **Port forwarding**
    - `kubectl port-forward svc/app-svc 8080:80`
    - Access in local brower(if using ec2): `ssh -i <key.pem> -L 8080:localhost:8080 ubuntu@<EC2-Public-IP>
`

kubectl rollout history deployment/test-deployment
kubectl rollout undo deployment/test-deployment  --to-revision=1



