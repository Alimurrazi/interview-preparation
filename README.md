# interview-preparation

# Kubernetes

* [What Is Kubernetes? What You Need To Know As A Developer](https://www.qovery.com/blog/what-is-kubernetes/)
* [What is Kubernetes?](https://phoenixnap.com/kb/what-is-kubernetes)
* [https://phoenixnap.com/kb/understanding-kubernetes-architecture-diagrams](https://phoenixnap.com/kb/understanding-kubernetes-architecture-diagrams)

# Kubernetes Interview Questions & Concepts

## Basic Concepts

### What is Kubernetes, and why is it used?
**Definition:** Kubernetes is an orchestration platform for containerized applications that automates deployment, scaling, and management.

**Key Benefits:**
- Automatic scaling
- Self-healing capabilities
- Automated rollouts and rollbacks

> **Tip:** Kubernetes works closely with containerization technologies like Docker.

### What is a Pod in Kubernetes?
A **Pod** is the smallest deployable unit in Kubernetes. It can run one or more containers that share networking and storage.

> **Tip:** A Pod is a higher-level abstraction over containers.

### What’s the difference between a Pod and a Deployment?
- A **Pod** is a single runtime instance of a containerized application.
- A **Deployment** manages multiple Pod replicas and ensures the desired state is maintained.

> **Tip:** A Deployment manages Pods through a ReplicaSet.

### What is a Kubernetes Cluster?
A **Kubernetes Cluster** is a group of nodes that run containerized applications. It consists of:
- **Control Plane:** Manages cluster state.
- **Worker Nodes:** Run application workloads.

> **Tip:** Control plane nodes manage scheduling, scaling, and networking.

### What’s the role of the Control Plane in Kubernetes?
The **Control Plane** manages the cluster and includes:
- **API Server:** Handles all cluster communication.
- **Scheduler:** Assigns workloads to nodes.
- **Controller Manager:** Ensures desired state (e.g., replicas, jobs).
- **etcd:** Stores cluster configuration and state.

---
## Architecture and Components

### What are the main components of a Kubernetes Node?
- **Kubelet:** Runs and manages Pods on a node.
- **Kube-Proxy:** Handles networking.
- **Container Runtime:** Runs containerized workloads (e.g., containerd, CRI-O).

### What is a Service in Kubernetes, and why do we need it?
A **Service** provides a stable network endpoint for accessing Pods. It supports:
- **ClusterIP:** Internal service.
- **NodePort:** Exposes service on a static port.
- **LoadBalancer:** Integrates with cloud provider load balancers.

### What’s the difference between a StatefulSet and a Deployment?
- **Deployment:** For stateless apps (e.g., web servers).
- **StatefulSet:** For stateful apps (e.g., databases) with persistent identity.

### What is an Ingress, and how does it differ from a Service?
- **Ingress** manages external HTTP/HTTPS traffic and routes it to services based on rules.
- **Service** routes traffic to Pods internally.

> **Tip:** Ingress requires an Ingress Controller like NGINX.

### What is etcd, and why is it important in Kubernetes?
**etcd** is a distributed key-value store that holds cluster state and configuration.

> **Tip:** It is the **“source of truth”** for Kubernetes.

---
## Deployment and Management

### How do you deploy an application to Kubernetes?
- Define a **YAML** file (Pod/Deployment spec).
- Apply it using `kubectl apply -f file.yaml`.

### What’s the purpose of a ReplicaSet?
A **ReplicaSet** ensures a specified number of identical Pod replicas are running.

> **Tip:** Deployments use ReplicaSets under the hood.

### How does Horizontal Pod Autoscaling (HPA) work?
HPA scales Pods based on resource metrics (CPU, memory, custom metrics).

> **Tip:** HPA depends on the Kubernetes Metrics Server.

### How do you update an application in Kubernetes without downtime?
Use **rolling updates**:
- Update the **Deployment** spec.
- Apply changes (`kubectl apply`).

> **Tip:** Configure `maxUnavailable` and `maxSurge` for fine control.

### What’s a ConfigMap, and how do you use it?
A **ConfigMap** stores configuration data (key-value pairs) that can be injected into Pods as environment variables or volumes.

> **Tip:** Use **Secrets** for sensitive data instead.

---
## Networking and Storage

### How does Kubernetes handle networking between Pods?
Each Pod gets a unique IP, and **CNI (Container Network Interface)** plugins (Flannel, Calico) manage networking.

### What is a PersistentVolume (PV) and PersistentVolumeClaim (PVC)?
- **PV:** Cluster-wide storage resource.
- **PVC:** Request by a Pod for storage.

### How do Services load balance traffic to Pods?
**Kube-Proxy** uses **IPTables/IPVS** to distribute traffic (default: round-robin).

### What’s the difference between ClusterIP, NodePort, and LoadBalancer Service types?
- **ClusterIP:** Internal communication.
- **NodePort:** Exposes service on a node’s port.
- **LoadBalancer:** Uses an external cloud provider’s load balancer.

### How do you expose an application externally in Kubernetes?
Use a **LoadBalancer Service** or **Ingress**.

---
## Troubleshooting and Operations

### How do you check the status of Pods in a cluster?
```sh
kubectl get pods
kubectl describe pod <name>
```

### What would you do if a Pod is stuck in `CrashLoopBackOff`?
- Check logs: `kubectl logs <pod>`
- Describe Pod: `kubectl describe pod <name>`
- Verify configuration, resource limits, and image validity.

### How do you scale a Deployment manually?
```sh
kubectl scale deployment <name> --replicas=5
```

> **Tip:** Compare manual scaling with HPA.

### What happens if a node in the cluster fails?
- The control plane reschedules Pods to healthy nodes.
- If using **ReplicaSets**, new Pods are created automatically.

### How do you view logs of a running Pod?
```sh
kubectl logs <pod-name> [-c <container>]
```
> **Tip:** Add `--follow` for real-time logs.

---
## Advanced Topics

### What’s the difference between Docker and a container runtime like containerd?
- **Docker** provides both containerization and extra tooling.
- **containerd** is a lightweight runtime used by Kubernetes.

### How does Kubernetes ensure high availability?
- Multiple control plane nodes.
- Replicated etcd.
- Pods spread across multiple nodes.

### What are Namespaces, and why are they useful?
Namespaces provide logical separation in a cluster (e.g., dev, prod).

### What is RBAC, and how do you implement it in Kubernetes?
RBAC (Role-Based Access Control) defines permissions using **Roles** and **RoleBindings**.

Example YAML for read-only access:
```yaml
apiVersion: rbac.authorization.k8s.io/v1
kind: Role
metadata:
  namespace: default
  name: read-only-role
rules:
- apiGroups: [""]
  resources: ["pods"]
  verbs: ["get", "list"]
```

### How would you handle a multi-tenant Kubernetes cluster?
- Use **Namespaces** for isolation.
- Apply **RBAC, NetworkPolicies, and ResourceQuotas**.

---
## Practical Scenarios

### Design a Kubernetes setup for a web app with a frontend and backend.
- **Frontend Deployment + Service (LoadBalancer)**
- **Backend Deployment + Service (ClusterIP)**
- **PersistentVolumeClaim for database storage**

### How would you handle a sudden traffic spike to your app?
- Use **HPA** for auto-scaling.
- Pre-scale manually if anticipating high traffic.

### How do you monitor a Kubernetes cluster?
- **kubectl top** for basic metrics.
- **Prometheus + Grafana** for advanced monitoring.
- **Alerts** for resource spikes.

