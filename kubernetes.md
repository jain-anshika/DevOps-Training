
# Kubernetes Training Notes ☸️

Kubernetes (K8s) is a **container orchestration platform** that automates **deployment, scaling, and management** of containerized applications.

---

## 🔹 Kubernetes Architecture

- **Master Node (Control Plane):**
  - API Server → Handles requests
  - etcd → Cluster state storage
  - Controller Manager → Ensures desired state
  - Scheduler → Assigns pods to nodes

- **Worker Node:**
  - Kubelet → Communicates with master
  - Kube-Proxy → Networking & load balancing
  - Container Runtime → Runs containers (Docker, containerd)

---

## 🔹 Basic Objects

### 1. Pod  
- Smallest deployable unit in Kubernetes.  
```yaml
apiVersion: v1
kind: Pod
metadata:
  name: nginx-pod
spec:
  containers:
    - name: nginx
      image: nginx
      ports:
        - containerPort: 80
````

```bash
kubectl apply -f pod.yaml
kubectl get pods
kubectl delete pod nginx-pod
```

---

### 2. Deployment

* Manages replicas of pods.

```yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: nginx-deploy
spec:
  replicas: 3
  selector:
    matchLabels:
      app: nginx
  template:
    metadata:
      labels:
        app: nginx
    spec:
      containers:
        - name: nginx
          image: nginx:latest
          ports:
            - containerPort: 80
```

```bash
kubectl apply -f deployment.yaml
kubectl get deployments
kubectl scale deployment nginx-deploy --replicas=5
kubectl rollout status deployment nginx-deploy
```

---

### 3. Service

* Exposes Pods to internal/external traffic.

**ClusterIP (default – internal only):**

```yaml
apiVersion: v1
kind: Service
metadata:
  name: nginx-service
spec:
  type: ClusterIP
  selector:
    app: nginx
  ports:
    - port: 80
      targetPort: 80
```

**NodePort (external access):**

```yaml
apiVersion: v1
kind: Service
metadata:
  name: nginx-nodeport
spec:
  type: NodePort
  selector:
    app: nginx
  ports:
    - port: 80
      targetPort: 80
      nodePort: 30080
```

---

## 🔹 Useful kubectl Commands

```bash
# Cluster info
kubectl cluster-info
kubectl get nodes

# Pods
kubectl get pods -o wide
kubectl describe pod <pod-name>

# Deployments
kubectl get deployments
kubectl rollout undo deployment nginx-deploy

# Services
kubectl get svc
kubectl describe svc nginx-service
```

---

## 🔹 Real-world Use Case (CI/CD with Jenkins + Kubernetes)

1. Jenkins pulls code from GitHub.
2. Jenkins builds Docker image & pushes to registry (DockerHub/ECR).
3. Jenkins updates Kubernetes Deployment (via `kubectl apply`).
4. Kubernetes ensures app scaling & availability.

---

# ✅ Key Notes

* Master = brains of the cluster
* Worker = runs application pods
* Deployment = manages pod replicas
* Service = exposes app internally or externally

---


