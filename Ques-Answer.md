- [Project](#Project)
  - [Architecture](#Pro-Architecture)
- [Networking](#Networking)

---
# Project

## Pro-Architecture

Great — you’ve asked an **important real-world DevOps question**:
👉 *“How does communication happen inside an OpenShift application running on AWS — from pod-to-pod, service-to-pod, and external access?”*

Since you already understand your project components (**Calico, HAProxy Router, AWS infrastructure, OCP deployments**), I will give you a **clear real-world architecture flow** EXACTLY matching how OpenShift works in enterprise setups.

---

# ✅ **High-Level Architecture (Your Project)**

You said:

* OpenShift cluster running **on AWS**
* Networking CNI is **Calico**
* External routing via **HAProxy Router** (OpenShift default Ingress Controller)
* Application in namespace `app-dev`
* Deployment has **4 pods**

So communication happens in three layers:

---

# 🧩 **1️⃣ Pod-to-Pod Networking (Inside Cluster) – Calico CNI**

Calico provides:

✔ Pod IP assignment
✔ Route distribution
✔ NetworkPolicies
✔ Pod-to-pod connectivity across nodes

### **How it works**

* Each pod gets its own IP from the **Calico pod CIDR**.
* Calico installs **routes** on each node (BGP or VXLAN).
* When Pod A wants to talk to Pod B:

  * Pod A sends traffic → virtual eth (`eth0`) → node
  * Node checks Calico route table
  * If Pod B is on same node → direct veth pair
  * If Pod B is on different node → VXLAN/Calico encapsulates packet and routes it to the target node

### **Key Point**

> **Pod-to-pod communication does NOT go through HAProxy or AWS Load Balancers. It is pure CNI-based routing handled by Calico.**

---

# 🧩 **2️⃣ Service-to-Pod Networking — OpenShift Services**

Your deployment will have a **Service** (ClusterIP type).

### What ClusterIP does:

* Creates a **virtual IP inside the cluster**
* Uses **iptables/IPVS** for load balancing across 4 pods

### Traffic flow:

```
Other Pods → ClusterIP Service → IPVS (kube-proxy) → One of the 4 pods
```

### Load balancing:

Round-robin or IPVS algorithm.

### Key Point:

> **Service ensures stable access even if pods restart or move.**

---

# 🧩 **3️⃣ External Traffic — HAProxy Router (Ingress Controller)**

OpenShift uses:

✔ `router-default` (HAProxy) in `openshift-ingress` namespace
✔ Routes → Ingress
✔ AWS Load Balancer (NLB/ELB) in front of Ingress pods

### **How external traffic enters OpenShift**

Flow:

```
User → AWS Load Balancer → HAProxy Route → Service → Pod
```

Step-by-step:

1. **User hits application URL** (e.g., [https://app.example.com](https://app.example.com))
2. DNS points to **AWS Load Balancer** created for OpenShift Ingress
3. AWS LB forwards traffic to **HAProxy Ingress Controller pods**
4. HAProxy looks at the route:

   * Host: `app-dev.example.com`
   * Path: `/`
5. HAProxy forwards to **ClusterIP service** of your app
6. Service → routes to one of the 4 pods

---

# 🧩 **4️⃣ Internal Traffic Between Services (Microservices Communication)**

If one pod in `app-dev` wants to call another service:

```
Pod → Service DNS name → ClusterIP → IPVS → Target Pod
```

Example DNS names:

```
myservice.app-dev.svc.cluster.local
```

OpenShift internal DNS (CoreDNS) resolves this.

---

# 🧩 **5️⃣ How AWS Networking Fits In**

Although OpenShift sits on AWS, internal networking works like Kubernetes.

AWS comes into picture only for:

### **Nodes networking**

* Nodes run in AWS VPC subnets
* Pod network (Calico) overlays on top of AWS VPC
* Node-to-node traffic happens via AWS VPC routing

### **External Access**

* AWS Load Balancer → OpenShift Router

### **Internal S3, RDS, Vault, etc**

Pods will access AWS services via:

* VPC routing
* Security groups
* IAM roles (IRSA/STS)

---

# 🎯 **Putting Everything Together — Full Architecture**

Here is your application’s communication flow:

```
                           ┌───────────────────────────────┐
                           │           AWS VPC              │
                           └───────────────────────────────┘
                                         │
                                 AWS Load Balancer
                                         │
                                (Ingress Traffic)
                                         │
                                 HAProxy Router
                      (openshift-ingress namespace pods)
                                         │
                           ┌────────────────────────┐
                           │  ClusterIP Service    │
                           └────────────────────────┘
                             /     /      /      \
                            v     v      v        v
                         POD1  POD2   POD3     POD4
                   (Calico CNI handles all routing)
```

---

# 🧠 **Interview-Level Summary**

> In OpenShift running on AWS, pod networking is handled by **Calico CNI**, which assigns IPs to pods and creates routes for cross-node communication. Services provide stable virtual IPs and load balance traffic using IPVS. External traffic enters through an **AWS Load Balancer**, reaches the **OpenShift HAProxy Router**, and then flows to the service → pods. Internal communication uses DNS (`service.namespace.svc`) and ClusterIP. Calico ensures secure, fast pod-to-pod connectivity inside the cluster.

---

# If you want

I can also draw:

✔ Pod-to-pod traffic flow
✔ Service-to-pod diagram
✔ End-to-end traffic flow diagram
✔ AWS load balancer + router architecture diagram

Just tell me **“give diagram”**.


A **CNI** stands for **Container Network Interface**.

It is a **standard/specification** that defines **how containers (pods) get network connectivity** in Kubernetes or OpenShift.

Think of CNI as the **plugin responsible for giving every pod an IP address and enabling communication between pods**.

---

# ✅ **Simple Definition**

**CNI = Networking plugin for Kubernetes pods**
It decides:

* How pods get IP addresses
* How pods communicate across nodes
* How routing happens
* How NetworkPolicies are enforced

---

# 🔧 **Why does Kubernetes need CNI?**

By default, Kubernetes has **no built-in way** to provide network connectivity to containers.

So Kubernetes depends on **CNI plugins** to do all networking.

---

# 🔥 Common CNI Plugins (Including Your Project)

| CNI Plugin      | Purpose                                              |
| --------------- | ---------------------------------------------------- |
| **Calico**      | L3 routing, NetworkPolicies (your project uses this) |
| **Flannel**     | Simple overlay network                               |
| **Cilium**      | eBPF-based high-performance networking               |
| **Weave Net**   | Mesh-based overlay                                   |
| **AWS VPC CNI** | Native pod networking in AWS EKS                     |

---

# 🧩 **How CNI Works (High Level)**

When a pod is created:

1. Kubelet calls the CNI plugin
2. CNI plugin:

   * Creates a **veth pair** (virtual ethernet)
   * Assigns an **IP address** to the pod
   * Adds **routes** so other pods can reach this pod
3. Updates Kubernetes network state

This is how all pods get unique IPs.

---

# 🧠 **Interview-Level Explanation**

> CNI (Container Network Interface) is a plugin-based networking system used by Kubernetes. CNI ensures every pod gets an IP address and sets up routing so pods can communicate across nodes. OpenShift uses Calico as its CNI, meaning Calico manages pod networking, routing, and network policies.

---

# 📘 **Example from Your Project**

You said you use **Calico**.

So Calico is the **CNI plugin**.

Meaning:

* Calico assigns pod IPs
* Calico routes traffic between nodes
* Calico applies NetworkPolicies
* Calico provides pod-level firewalling

---

If you want, I can also explain:

✔ CNI vs CSI vs CRI
✔ Detailed pod networking diagram
✔ How Calico works internally (VXLAN/BGP)


---


# Networking

1. Frequently used ports in Devops environment ?

- SSH (22, TCP): Secure remote login to servers and automation access.
- HTTP (80, TCP) and HTTPS (443, TCP): Standard web traffic ports.
- Jenkins (8080, TCP): Jenkins server web UI.
- Docker Registry (5000, TCP): Private container registry communication.
- Kubernetes API Server (6443, TCP): Kubernetes cluster control plane communication.
- Prometheus (9090, TCP): Monitoring and alerting toolkit.
- Git (9418, TCP): Git protocol for version control.
- RDP (3389, TCP): Remote Desktop Protocol for Windows servers.
- SMTP (25, TCP): Email sending for notifications.
- Vault (8200, TCP): HashiCorp Vault secrets management.

