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

### **Different Workload Resources in Kubernetes**
| Resource                  | Use Case                                |
| ------------------------- | --------------------------------------- |
| **Deployment**            | Stateless apps, rolling updates         |
| **ReplicaSet**            | Keep pods running (used by Deployment)  |
| **StatefulSet**           | Stateful apps, databases, ordered pods  |
| **DaemonSet**             | Run pod on every node (logging/monitor) |
| **Job**                   | One-time tasks, batch processing        |
| **CronJob**               | Scheduled tasks                         |


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



