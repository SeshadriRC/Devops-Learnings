- Install metric server

kubectl get deployment -n kube-system | grep metrics
kubectl apply -f https://github.com/kubernetes-sigs/metrics-server/releases/latest/download/components.yaml


kubectl edit deployment metrics-server -n kube-system

Find the container section and modify like this

containers:
- name: metrics-server
  image: k8s.gcr.io/metrics-server/metrics-server:v0.6.3
  args:
    - --kubelet-insecure-tls
    - --kubelet-preferred-address-types=InternalIP
 
---
Great! I’ll give you a **full practical HPA (Horizontal Pod Autoscaler)** lab that works perfectly on a **2-node Kubernetes cluster** (like in KillerKoda, KinD, or local VMs).

You will get:

✅ Deployment YAML (with CPU requests)
✅ Service YAML
✅ HPA YAML
✅ Load generator (busybox stress test)
✅ Step-by-step verification
❗ Notes about common mistakes

Let’s start.

**Chatgpt**
---

# 🚀 **1. Create a Deployment that HPA can scale**

HPA **requires CPU or memory requests** to calculate metrics.
Below deployment uses Nginx + CPU requests.

### **deployment.yaml**

```yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: hpa-nginx
spec:
  replicas: 1
  selector:
    matchLabels:
      app: hpa-nginx
  template:
    metadata:
      labels:
        app: hpa-nginx
    spec:
      containers:
        - name: nginx
          image: nginx
          ports:
            - containerPort: 80
          resources:
            requests:
              cpu: 100m
            limits:
              cpu: 200m
```

Apply:

```
kubectl apply -f deployment.yaml
```

---

# 🚀 **2. Create a Service for access (ClusterIP is enough)**

### **service.yaml**

```yaml
apiVersion: v1
kind: Service
metadata:
  name: hpa-nginx-svc
spec:
  selector:
    app: hpa-nginx
  ports:
    - port: 80
      targetPort: 80
```

Apply:

```
kubectl apply -f service.yaml
```

---

# 🚀 **3. Create the HPA resource**

This HPA will scale 1 → 5 pods based on CPU usage.

### **hpa.yaml**

```yaml
apiVersion: autoscaling/v2
kind: HorizontalPodAutoscaler
metadata:
  name: hpa-nginx
spec:
  scaleTargetRef:
    apiVersion: apps/v1
    kind: Deployment
    name: hpa-nginx
  minReplicas: 1
  maxReplicas: 5
  metrics:
    - type: Resource
      resource:
        name: cpu
        target:
          type: Utilization
          averageUtilization: 50
```

Apply:

```
kubectl apply -f hpa.yaml
```

Check HPA:

```
kubectl get hpa
```

---

# 🚀 **4. Install Metrics Server (MANDATORY)**

If metrics-server is not running, HPA will NOT work.

Check:

```
kubectl get deployment -n kube-system | grep metrics
```

If missing, install:

```
kubectl apply -f https://github.com/kubernetes-sigs/metrics-server/releases/latest/download/components.yaml
```

(For Killerkoda, usually it's already installed.)

---

# 🚀 **5. Generate LOAD to trigger HPA**

Find the ClusterIP:

```
kubectl get svc hpa-nginx-svc
```

Open a load-generating pod:

```
kubectl run -it load-generator --image=busybox --restart=Never -- /bin/sh
```

Run infinite curl loop:

```
while true; do wget -q -O- http://hpa-nginx-svc.default.svc.cluster.local; done
while true; do wget -O- http://hpa-nginx-svc.default.svc.cluster.local > /dev/null; done &

```

This will hammer the nginx service → CPU goes high → HPA scales.

---

# 🚀 **6. Watch autoscaling**

New terminal:

```
kubectl get hpa -w
```

And also:

```
kubectl get pods -w
```

You will see:

✅ CPU ↑
➡️ HPA scale up: 1 → 3 → 5 pods
⏳ When load stops → pods scale down again

---

# 🚀 **7. STOP load**

Go back to load pod and exit the loop (Ctrl+C)
Then delete load pod:

```
kubectl delete pod load-generator
```

Watch auto-scale-down (slow—takes ~5 mins).

---

# ⚠ COMMON ISSUES

### ❗ 1. HPA shows “unknown” CPU

→ Metrics-server not installed or no CPU requests in Deployment.

### ❗ 2. Pods not scaling over 1

→ You applied HPA to wrong deployment name.

### ❗ 3. CPU always shows 0%

→ No traffic hitting service.

### ❗ 4. Node capacity insufficient

→ Set lower limits, e.g. requests: 50m.

---

# 🎁 Do you want:

✅ HPA with **Memory** metrics
✅ HPA with **custom metrics** (Prometheus Adapter)
✅ HPA with **KEDA**
✅ Stress testing via **Apache Benchmark (ab)**
✅ Load simulation using **wrk / hey**

Just tell me!

---



