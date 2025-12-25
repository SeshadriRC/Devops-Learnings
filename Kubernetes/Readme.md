## Notes

https://notes.kodekloud.com/docs/CKA-Certification-Course-Certified-Kubernetes-Administrator/Introduction/Course-Introduction

Git: https://github.com/SeshadriRC/certified-kubernetes-administrator-course

**Pending**

- kubernetes the hard way - 255 ( https://cognizant.udemy.com/course/certified-kubernetes-administrator-with-practice-tests/learn/lecture/22498956#overview )
- 


# Service

**NodePort**
- External traffic hits the node IP → NodePort → Service → Pods.


# Activities

- [Install Calico CNI](#Install-Calico-CNI)
- [Cluster-installation](#cluster-installation)
- [Install-helm](#install-helm)



# Concepts

- [Networking](#networking)
  - [DNS](#DNS)
  - [Network-namespace](#network-namespace)
  - [Nework-lab](#network-lab)
  - [CNI-lab](#cni-lab)
  - [Service-networking-lab](#service-lab)
  - [Core-DNS-Lab](#core-dns-lab)
  - [Ingress-networking-lab1](#Ingress-lab1)
  - [Ingress-networking-lab2](#Ingress-lab2)
  - [Gateway-api](#gateway-api-lab)



# Networking

## DNS

```
cat /etc/hosts
cat /etc/resolv.conf
cat /etc/nsswitch.conf

nameserver 8.8.8.8
```

## Network namespace

```
ip netns add red
ip netns

ip link  -> need to see
ip netns exec red ip link

arp
iptables -nVL -t nat
```

---
## Network-lab

1. What is the network interface configured for cluster connectivity on the controlplane node?

node-to-node communication

Run the ip a / ip link command and identify the interface.

```
Run: kubectl get nodes -o wide to see the IP address assigned to the controlplane node.

controlplane:~# kubectl get nodes controlplane -o wide
NAME           STATUS   ROLES           AGE   VERSION   INTERNAL-IP   EXTERNAL-IP   OS-IMAGE             KERNEL-VERSION   CONTAINER-RUNTIME
controlplane   Ready    control-plane   7m    v1.29.0   192.23.97.3   <none>        Ubuntu 22.04.3 LTS   5.4.0-1106-gcp   containerd://1.6.26

controlplane:~#
In this case, the internal IP address used for node to node communication is 192.23.97.3.

Important Note : The result above is just an example, the node IP address will vary for each lab.

Next, find the network interface to which this IP is assigned by making use of the ip a command:

controlplane:~# ip a | grep -B2 192.23.97.3
25556: eth0@if25557: <BROADCAST,MULTICAST,UP,LOWER_UP> mtu 1450 qdisc noqueue state UP group default 
    link/ether 02:42:c0:17:61:03 brd ff:ff:ff:ff:ff:ff link-netnsid 0
    inet 192.23.97.3/24 brd 192.23.97.255 scope global eth0

controlplane:~#
Here you can see that the interface associated with this IP is eth0 on the host.

Ans:
controlplane ~ ➜  ip a | grep -B2 192.168.227.144
3: eth0@if35791: <BROADCAST,MULTICAST,UP,LOWER_UP> mtu 1410 qdisc noqueue state UP group default 
    link/ether 3e:f8:ac:fc:9c:31 brd ff:ff:ff:ff:ff:ff link-netnsid 0
    inet 192.168.227.144/32 scope global eth0

```

2. What is the MAC address of the interface on the controlplane node?

```
see using below command

ip link show eth0
```

This is the **Linux network interface details** for `eth0`.
Let me explain **each part clearly**, line by line 👇

---

## Command

```bash
ip link show eth0
```

➡️ Shows **Layer-2 (link layer)** information of the `eth0` network interface.

---

## Output explained

```text
3: eth0@if35791:
```

### 🔹 `3`

* Interface **index number** (internal kernel ID)

### 🔹 `eth0`

* Interface name inside this system (often inside a **container / VM**)

### 🔹 `@if35791`

* `eth0` is **linked to another interface**
* Usually means:

  * **veth pair**
  * Common in **Docker / Kubernetes / network namespaces**
* `if35791` exists in the **host or another namespace**

👉 Very common inside containers.

---

```text
<BROADCAST,MULTICAST,UP,LOWER_UP>
```

### Interface flags

| Flag      | Meaning                       |
| --------- | ----------------------------- |
| BROADCAST | Can send broadcast packets    |
| MULTICAST | Supports multicast traffic    |
| UP        | Interface is enabled          |
| LOWER_UP  | Physical/link layer is active |

✅ Interface is **working and connected**

---

```text
mtu 1410
```

### 🔹 MTU (Maximum Transmission Unit)

* Max packet size = **1410 bytes**
* Default Ethernet MTU is **1500**
* Reduced MTU usually indicates:

  * Overlay network (Docker, Kubernetes, VXLAN)
  * Cloud networking

---

```text
qdisc noqueue
```

### 🔹 Queueing discipline

* `noqueue` → no packet queuing
* Typical for:

  * Virtual interfaces
  * Containers
  * veth devices

---

```text
state UP
```

* Interface state is **UP**
* Matches flags above

---

```text
mode DEFAULT
```

* Normal operating mode

---

```text
group default
```

* Interface belongs to default group

---

```text
link/ether 3e:f8:ac:fc:9c:31
```

### 🔹 MAC Address

* Hardware (virtual) MAC of `eth0`

---

```text
brd ff:ff:ff:ff:ff:ff
```

* Broadcast MAC address

---

```text
link-netnsid 0
```

### 🔹 Network Namespace ID

* Interface is connected to **another network namespace**
* `0` means first linked namespace
* Strong indicator of:

  * Docker container
  * Kubernetes pod

---

## 📌 Summary (Interview-friendly)

> This output shows a **virtual Ethernet interface (`eth0`)**, most likely inside a **container or Kubernetes pod**.
> The `@ifXXXX` and `link-netnsid` indicate a **veth pair connected to another network namespace**.
> The interface is **UP**, has a **reduced MTU**, and uses `noqueue`, which is typical for container networking.

---

If you want, I can also explain:

* How `veth pairs` work
* Difference between `eth0@ifXXXX` vs normal `eth0`
* How this looks on the **host side**

Just tell me 👍

3. We use Containerd as our container runtime. What is the interface/bridge created by Containerd on the controlplane node?

Note: If you have SSH'd into node01, you can return to the controlplane node by typing: exit.
This will close the SSH session and bring you back to the main controlplane terminal.

**sol**
Look for a network interface that is a bridge created by Containerd.
It usually starts with cni (for example, cni0) and is not eth0 or docker0. This is the interface used for container networking on the controlplane node.

```
ip link

controlplane ~ ➜  ip link
1: lo: <LOOPBACK,UP,LOWER_UP> mtu 65536 qdisc noqueue state UNKNOWN mode DEFAULT group default qlen 1000
    link/loopback 00:00:00:00:00:00 brd 00:00:00:00:00:00
3: eth0@if35791: <BROADCAST,MULTICAST,UP,LOWER_UP> mtu 1410 qdisc noqueue state UP mode DEFAULT group default 
    link/ether 3e:f8:ac:fc:9c:31 brd ff:ff:ff:ff:ff:ff link-netnsid 0
4: flannel.1: <BROADCAST,MULTICAST,UP,LOWER_UP> mtu 1360 qdisc noqueue state UNKNOWN mode DEFAULT group default 
    link/ether 8e:0c:b6:01:6b:4f brd ff:ff:ff:ff:ff:ff
5: cni0: <BROADCAST,MULTICAST,UP,LOWER_UP> mtu 1360 qdisc noqueue state UP mode DEFAULT group default qlen 1000
    link/ether 62:f1:96:6f:f2:1d brd ff:ff:ff:ff:ff:ff
6: veth035ef1d1@if2: <BROADCAST,MULTICAST,UP,LOWER_UP> mtu 1360 qdisc noqueue master cni0 state UP mode DEFAULT group default qlen 1000
    link/ether 8a:34:19:9e:2a:11 brd ff:ff:ff:ff:ff:ff link-netns cni-80f30a34-b52c-d142-89cf-0e71cbfd0d3f
7: veth37844397@if2: <BROADCAST,MULTICAST,UP,LOWER_UP> mtu 1360 qdisc noqueue master cni0 state UP mode DEFAULT group default qlen 1000
    link/ether c6:f7:97:bd:bb:86 brd ff:ff:ff:ff:ff:ff link-netns cni-770947e0-6cd9-1dbb-c5c0-0165f098397a
```

4. What is the state of the interface cni0?

Run the command: ip link show cni0 and look for the state.
```
ip link show cni0

controlplane ~ ➜  ip link show cni0
5: cni0: <BROADCAST,MULTICAST,UP,LOWER_UP> mtu 1360 qdisc noqueue state UP mode DEFAULT group default qlen 1000
    link/ether 62:f1:96:6f:f2:1d brd ff:ff:ff:ff:ff:ff
```

5. If you were to ping google from the controlplane node, which route does it take?

What is the IP address of the Default Gateway?

Run the command: ip route show default and look at for default gateway.

```
controlplane ~ ➜  ip route show default
default via 169.254.1.1 dev eth0 

controlplane ~ ➜  ip route
default via 169.254.1.1 dev eth0 
169.254.1.1 dev eth0 scope link 
172.17.0.0/24 dev cni0 proto kernel scope link src 172.17.0.1 
172.17.1.0/24 via 172.17.1.0 dev flannel.1 onlink 

```

This is the **Linux routing table** of your system (very likely a **Kubernetes control plane or node**).
Let’s break it down **line by line**, from basics 👇

---

## Command

```bash
ip route
```

➡️ Shows the **Layer-3 routing table** (how packets are forwarded).

---

## Output explained

```text
default via 169.254.1.1 dev eth0
```

### 🔹 Default route

* Used when destination IP **does not match any specific route**
* Traffic goes:

  * **Gateway**: `169.254.1.1`
  * **Interface**: `eth0`

📌 This is how the node reaches **outside networks / internet / cluster API**

---

```text
169.254.1.1 dev eth0 scope link
```

### 🔹 Link-local gateway route

* `169.254.0.0/16` is **link-local IP range**
* Means:

  * Gateway is **directly reachable** on `eth0`
  * No ARP forwarding beyond this link

📌 Very common in:

* Cloud VMs
* Kubernetes nodes
* CNI setups

---

```text
172.17.0.0/24 dev cni0 proto kernel scope link src 172.17.0.1
```

### 🔹 Pod network (local)

* Pod CIDR: `172.17.0.0/24`
* Interface: `cni0` (bridge created by CNI)
* Source IP: `172.17.0.1` (node’s bridge IP)

📌 Pods on **this node** get IPs from this range.

---

```text
172.17.1.0/24 via 172.17.1.0 dev flannel.1 onlink
```

### 🔹 Remote pod network (other node)

* Pod CIDR of **another node**
* Uses:

  * Interface: `flannel.1` (overlay VXLAN)
  * `onlink` → next hop is reachable via overlay, not ARP

📌 Traffic to pods on other nodes is encapsulated by **Flannel VXLAN**.

---

## 🧠 Big picture (very important)

| Component   | Purpose                         |
| ----------- | ------------------------------- |
| eth0        | Node ↔ external / cloud network |
| cni0        | Local pod bridge                |
| flannel.1   | Overlay network to other nodes  |
| 169.254.1.1 | Cloud-provided default gateway  |

---

## Interview-ready explanation 🎯

> This routing table belongs to a Kubernetes node using **Flannel CNI**.
> Local pod traffic goes through `cni0`, inter-node pod traffic goes through the `flannel.1` VXLAN interface, and external traffic exits via `eth0` using a link-local gateway.

---

If you want, next I can explain:

* Packet flow **pod → pod (same node)**
* Packet flow **pod → pod (different node)**
* Why `169.254.x.x` is used in Kubernetes/cloud

Just say the word 👍


6. What is the port the kube-scheduler is listening on in the controlplane node?



```
netstat --help

root@controlplane:~# netstat -nplt | grep scheduler
tcp        0      0 127.0.0.1:10259         0.0.0.0:*               LISTEN      3665/kube-scheduler 
root@controlplane:~# 
```

7. Notice that ETCD is listening on two ports. Which of these have more client connections established?


```
controlplane ~ ➜  netstat -nplt | grep etcd
tcp        0      0 127.0.0.1:2381          0.0.0.0:*               LISTEN      2725/etcd           
tcp        0      0 127.0.0.1:2379          0.0.0.0:*               LISTEN      2725/etcd           
tcp        0      0 192.168.227.144:2379    0.0.0.0:*               LISTEN      2725/etcd           
tcp        0      0 192.168.227.144:2380    0.0.0.0:*               LISTEN      2725/etcd           

netstat -anp | grep etcd
netstat -anp | grep etcd | grep 2379 | wc -l
65

Correct! That's because 2379 is the port of ETCD to which all control plane components connect to. 2380 is only for etcd peer-to-peer connectivity. When you have multiple controlplane nodes. In this case we don't.
```

That statement is explaining **why you are seeing etcd ports** and **which one actually matters in your setup**. Let’s decode it in **simple terms** 👇

---

## What the statement is telling you

### 🔹 **Port 2379**

> “2379 is the port of ETCD to which all control plane components connect to”

Meaning:

* **kube-apiserver** connects to etcd on **port 2379**
* This is the **client port**
* Kubernetes **reads/writes cluster state** via this port
* **Mandatory**, even in a single-node cluster

📌 Without 2379 → Kubernetes will **not work**

---

### 🔹 **Port 2380**

> “2380 is only for etcd peer-to-peer connectivity”

Meaning:

* Used **only between etcd members**
* Handles:

  * Leader election
  * Data replication
  * Raft consensus

📌 Required **only when you have multiple control plane / etcd nodes**

---

### 🔹 **“When you have multiple controlplane nodes. In this case we don't.”**

Meaning:

* Your cluster has **only ONE control-plane node**
* So:

  * No etcd peers exist
  * Port **2380 is not actively used**
  * But it may still be **open/listening** by default

---

## In short (one-glance view)

| Port     | Purpose                        | Used in your cluster? |
| -------- | ------------------------------ | --------------------- |
| **2379** | Client (kube-apiserver → etcd) | ✅ YES                 |
| **2380** | Peer (etcd ↔ etcd)             | ❌ NO (single node)    |

---

## Interview-ready explanation 🎯

> Port 2379 is the etcd client port used by Kubernetes control plane components like kube-apiserver. Port 2380 is used only for etcd peer communication in HA clusters. Since this is a single control-plane setup, peer communication is not required.

---

## Key takeaway 💡

✔ Seeing **2379 is essential**
✔ Seeing **2380 is normal but unused**
✔ Single-node control plane ≠ HA etcd

---

If you want, I can also explain:

* Why 2380 still listens in single-node
* What breaks if 2379 is blocked
* How HA control plane changes this

Just tell me 👍

---


The command:

```bash
netstat -anp | grep etcd
```

is used to **identify all network connections and listening ports related to the `etcd` process**.

Let’s break it down **clearly and simply** 👇

---

## What each option means

### `netstat`

Shows network connections, routing tables, and listening ports.

### `-a`

Shows **all sockets** (listening + established).

### `-n`

Shows **IP addresses and ports numerically** (no DNS resolution).

### `-p`

Shows the **process name and PID** using the socket.

### `| grep etcd`

Filters the output to show **only entries related to the `etcd` process**.

---

## What this command tells you

It shows:

* Which **ports etcd is listening on**
* Which **IPs it is bound to**
* Any **active connections** involving etcd
* The **PID** of the etcd process

Example output interpretation:

```text
tcp  0  0 127.0.0.1:2379   0.0.0.0:*   LISTEN   2725/etcd
```

➡️ etcd is:

* Using TCP
* Listening on port **2379**
* Bound to **localhost**
* Running as process **PID 2725**

---

## Why this is useful (real-world)

* Verify **etcd is running**
* Confirm **correct ports** (2379 / 2380 / 2381)
* Troubleshoot:

  * kube-apiserver can’t reach etcd
  * firewall / bind issues
* Security validation (bound to localhost vs node IP)

---

## Interview-ready explanation 🎯

> This command is used to check which ports etcd is listening on and which IP addresses they are bound to. The `-p` option helps identify the etcd process and its PID, which is useful for debugging connectivity and port issues.

---

## Modern alternative (FYI)

```bash
ss -lntp | grep etcd
```

(`ss` is faster and preferred on modern Linux)

---

If you want, I can also:

* Explain LISTEN vs ESTABLISHED states
* Show how kube-apiserver connects to etcd
* Map etcd ports to Kubernetes components

Just say 👍


**Short answer:**
👉 **etcd runs on TWO main ports**, not just one.

---

## Primary etcd ports (standard & correct)

| Port     | Purpose         | Used by                  |
| -------- | --------------- | ------------------------ |
| **2379** | **Client port** | kube-apiserver, etcdctl  |
| **2380** | **Peer port**   | etcd ↔ etcd (HA cluster) |

---

## Optional / additional port

| Port     | Purpose                     |
| -------- | --------------------------- |
| **2381** | Metrics & health (optional) |

---

## Simple explanation

* **2379** → “Clients talk *to* etcd”
* **2380** → “etcd nodes talk *among themselves*”
* **2381** → “Monitoring & health checks”

So etcd is **not limited to one port** — it **intentionally runs on multiple ports**.

---

## In Kubernetes (what you usually see)

```text
2379 → kube-apiserver → etcd
2380 → etcd ↔ etcd (leader election, replication)
2381 → Prometheus / health checks
```

---

## Interview-ready one-liner 🎯

> *etcd primarily runs on port **2379** for client communication and **2380** for peer communication in an HA setup. Port **2381** is optionally used for metrics and health checks.*

---

## If the interviewer insists: “Which ONE port?”

👉 Answer: **2379 (client port)**
because **Kubernetes cannot function without it**.

---

If you want, I can also explain:

* Which ports must be opened in firewall
* What happens if 2380 is blocked
* Single-node vs HA etcd behavior

Just tell me 👍

## CNI-lab

---

1. Inspect the kubelet service and identify the container runtime endpoint value is set for Kubernetes.

```
Run the following command:

cat /var/lib/kubelet/config.yaml | grep containerRuntimeEndpoint
The container runtime endpoint is unix:///var/run/containerd/containerd.sock.
```

2. What is the path configured with all binaries of CNI supported plugins?

```
The CNI binaries are located under /opt/cni/bin by default.
```

3. Identify which of the below plugins is not available in the list of available CNI plugins on this host?

```
Run the command: ls /opt/cni/bin and identify the one not present at that directory.

controlplane ~ ➜  ls /opt/cni/bin
bandwidth  dhcp   firewall  host-device  ipvlan   loopback  portmap  README.md  static  tuning  vrf
bridge     dummy  flannel   host-local   LICENSE  macvlan   ptp      sbr        tap     vlan
```

4. What is the CNI plugin configured to be used on this kubernetes cluster?

```
Run the command: ls /etc/cni/net.d/ and identify the name of the plugin.

controlplane ~ ➜  ls /etc/cni/net.d/
10-flannel.conflist
```

5. What binary executable file will be invoked by the container runtime after a container and its associated namespace are created?

```
Look at the type field in file /etc/cni/net.d/10-flannel.conflist.

controlplane ~ ➜  cat /etc/cni/net.d/10-flannel.conflist | grep type
      "type": "flannel",
      "type": "portmap",

```

6. Which CNI plugin is currently installed on the cluster?

```
cat /etc/cni/net.d/
```

7. Two applications, frontend and backend, have been provisioned in the default namespace in the cluster. A network policy deny-backend has been provisioned to block traffic to the backend app. To test whether the policy is working or not, use the following command:

kubectl exec -it frontend -- curl -m 5 <BACKEND-POD-IP>

You need to replace <BACKEND-POD-IP> with the IP of the backend pod. To retrieve the IP:

kubectl get pods -o wide

Can the frontend pod reach the backend pod?

```
kubectl exec -it frontend -- curl -m 5 <BACKEND-POD-IP>

controlplane /etc/cni/net.d ✖ k get netpol
NAME           POD-SELECTOR   AGE
deny-backend   app=backend    4m29s

controlplane /etc/cni/net.d ➜  k describe netpol deny-backend
Name:         deny-backend
Namespace:    default
Created on:   2025-12-20 07:24:24 +0000 UTC
Labels:       <none>
Annotations:  <none>
Spec:
  PodSelector:     app=backend
  Allowing ingress traffic:
    <none> (Selected pods are isolated for ingress connectivity)
  Not affecting egress traffic
  Policy Types: Ingress

controlplane /etc/cni/net.d ➜  k get netpol deny-backend -o yaml
apiVersion: networking.k8s.io/v1
kind: NetworkPolicy
metadata:
  annotations:
    kubectl.kubernetes.io/last-applied-configuration: |
      {"apiVersion":"networking.k8s.io/v1","kind":"NetworkPolicy","metadata":{"annotations":{},"name":"deny-backend","namespace":"default"},"spec":{"ingress":[],"podSelector":{"matchLabels":{"app":"backend"}},"policyTypes":["Ingress"]}}
  creationTimestamp: "2025-12-20T07:24:24Z"
  generation: 1
  name: deny-backend
  namespace: default
  resourceVersion: "1595"
  uid: 2818c0a0-1634-4dce-89f3-b68d501148e5
spec:
  podSelector:
    matchLabels:
      app: backend
  policyTypes:
  - Ingress

controlplane /etc/cni/net.d ➜  k get pods -o wide
NAME       READY   STATUS    RESTARTS   AGE     IP           NODE           NOMINATED NODE   READINESS GATES
backend    1/1     Running   0          7m45s   172.17.0.5   controlplane   <none>           <none>
frontend   1/1     Running   0          7m45s   172.17.0.4   controlplane   <none>           <none>

controlplane /etc/cni/net.d ➜  k exec -it frontend -- sh
# curl
curl: try 'curl --help' or 'curl --manual' for more information
# curl -m 5 172.17.0.5
<!DOCTYPE html>
<html>
<head>
<title>Welcome to nginx!</title>
<style>
html { color-scheme: light dark; }
body { width: 35em; margin: 0 auto;
font-family: Tahoma, Verdana, Arial, sans-serif; }
</style>
</head>
<body>
<h1>Welcome to nginx!</h1>
<p>If you see this page, the nginx web server is successfully installed and
working. Further configuration is required.</p>

<p>For online documentation and support please refer to
<a href="http://nginx.org/">nginx.org</a>.<br/>
Commercial support is available at
<a href="http://nginx.com/">nginx.com</a>.</p>

<p><em>Thank you for using nginx.</em></p>
</body>
</html>
#

```
---

8. As you have seen, the curl command from the frontend app to the backend app succeeded and returned the NGINX welcome page. However, this should not be the expected behavior given that we have a deny-backend network policy in place to prohibit this from happening.
Why did the curl command succeed?

```
this cni does not support netpol
```

9. The Flannel CNI does not support NetworkPolicies.

Delete Flannel CNI.

```
To clean up the Flannel CNI from the cluster, delete the Flannel DaemonSet, configuration file, as well as the ConfigMap hosting the Flannel configuration:

kubectl delete daemonset -n kube-flannel kube-flannel-ds
kubectl delete cm kube-flannel-cfg -n kube-flannel
rm /etc/cni/net.d/10-flannel.conflist
```

---

10. Install Calico CNI

- https://docs.tigera.io/calico/latest/getting-started/kubernetes/self-managed-onprem/onpremises

# Install Calico CNI
    
Calico is a Container Network Interface (CNI) that provides support for Kubernetes NetworkPolicy enforcement. In this task, you will install Calico into the cluster to enable network policy functionality.

Installation Guidelines
Follow the self-managed on-premises installation guide for Calico, available from the links in the top-right corner of the terminal:

Calico Documentation (Latest)

Critical Requirements
Dataplane Selection:

Use the standard Linux dataplane (iptables-based)
Do NOT use the eBPF dataplane option — it is incompatible with this lab environment
Installation Components:

Calico is deployed using the Tigera Operator
You must install both the operator and the custom resource definitions (CRDs)
Follow the installation steps in the correct order as specified in the documentation
Network Configuration:

The pod network CIDR must be configured as 172.17.0.0/16
This setting is critical for pod-to-pod communication in this cluster
You will need to create and modify a Calico Installation custom resource manifest
Ensure the cidr field matches the required value before applying the configuration
Verification
Confirm that all Calico pods reach a Running state before proceeding:

watch kubectl get pods -n calico-system


Note: You may see errors on the csi-node-driver pod deployed in the calico-system namespace after you deploy Calico. These can be safely ignored and will not affect network policy enforcement.

```
First, install the Calico CRDs:

kubectl create -f https://raw.githubusercontent.com/projectcalico/calico/v3.31.0/manifests/operator-crds.yaml
Then install the Calico operator:

kubectl create -f https://raw.githubusercontent.com/projectcalico/calico/v3.31.0/manifests/tigera-operator.yaml
Download the custom resource definition yaml:

curl https://raw.githubusercontent.com/projectcalico/calico/v3.31.0/manifests/custom-resources.yaml -O
Edit the file to update the CIDR field to 172.17.0.0/16 for pod communication to work successfully on the cluster:

# Edit custom-resources.yaml and update the cidr field
Edit the file to update the CIDR network. The file should look like this:

apiVersion: operator.tigera.io/v1
kind: Installation
metadata:
  name: default
spec:
  calicoNetwork:
    ipPools:
    - name: default-ipv4-ippool
      blockSize: 26
      cidr: 172.17.0.0/16
      encapsulation: VXLANCrossSubnet
      natOutgoing: Enabled
      nodeSelector: all()
---
apiVersion: operator.tigera.io/v1
kind: APIServer
metadata:
  name: default
spec: {}
Apply the manifest file:

kubectl apply -f custom-resources.yaml
Wait until the calico pods are running:

watch kubectl get pods -A
You may ignore errors on the csi-node-driver pod deployed in the calico-system namespace.
```

---

11. After we deployed the Calico CNI, we redeployed the applications. Now, Let's re-test the connectivity from the frontend app to the backend app:

kubectl exec -it frontend -- curl -m 5 <BACKEND-POD-IP>

You need to replace <BACKEND-POD-IP> with the IP of the backend pod. To retrieve the IP:

kubectl get pods -o wide

Can the frontend pod reach the backend pod?

```
kubectl exec -it frontend -- curl -m 5 <BACKEND-POD-IP>

```

# Service-Lab

1. What is the IP address and subnet mask assigned to the controlplane node's primary network interface?

```
Run the command:

ip addr show eth0
Check the IP address and subnet mask assigned to the eth0 interface.

controlplane ~ ➜  ip addr show eth0
3: eth0@if2092: <BROADCAST,MULTICAST,UP,LOWER_UP> mtu 1410 qdisc noqueue state UP group default 
    link/ether 9a:b7:ff:93:4b:2a brd ff:ff:ff:ff:ff:ff link-netnsid 0
    inet 192.168.126.149/32 scope global eth0
       valid_lft forever preferred_lft forever
    inet6 fe80::98b7:ffff:fe93:4b2a/64 scope link 
       valid_lft forever preferred_lft forever

Ans: 192.168.126.149/32
```

2. What is the range of IP addresses configured for PODs on this cluster?

```
The network is configured with canal. Check the canal pods logs using the command kubectl logs <canal-pod-name> -n kube-system and look for default IPv4 pool range.

Or

Check the kube-controller-manager.yaml manifest file and look for the --cluster-cidr parameter, which defines the IP range assigned to Pods in the cluster.

controlplane ~ ➜  cat /etc/kubernetes/manifests/kube-controller-manager.yaml   | grep cluster-cidr
    - --cluster-cidr=172.17.0.0/16
```

3. What is the IP Range configured for the services within the cluster?

```
Inspect the setting on kube-api server by running on command:
cat /etc/kubernetes/manifests/kube-apiserver.yaml   | grep cluster-ip-range
```

4. How many kube-proxy pods are deployed in this cluster?

```
kubectl get pods -n kube-system
```

5. What type of proxy is the kube-proxy configured to use?

```
Check the logs of the kube-proxy pods. Run the command:
kubectl logs -n kube-system kube-proxy-bbt2c

controlplane ~ ➜  kubectl logs -n kube-system kube-proxy-bbt2c
I1221 03:23:15.500113       1 server_linux.go:53] "Using iptables proxy"
I1221 03:23:15.918928       1 shared_informer.go:349] "Waiting for caches to sync" controller="node informer cache"
I1221 03:23:16.019717       1 shared_informer.go:356] "Caches are synced" controller="node informer cache"
I1221 03:23:16.019762       1 server.go:219] "Successfully retrieved NodeIPs" NodeIPs=["192.168.126.149"]
I1221 03:23:16.093191       1 conntrack.go:60] "Setting nf_conntrack_max" nfConntrackMax=524288
E1221 03:23:16.094733       1 server.go:256] "Kube-proxy configuration may be incomplete or incorrect" err="nodePortAddresses is unset; NodePort connections will be accepted on all local IPs. Consider using `--nodeport-addresses primary`"
I1221 03:23:16.189271       1 server.go:265] "kube-proxy running in dual-stack mode" primary ipFamily="IPv4"
I1221 03:23:16.189324       1 server_linux.go:132] "Using iptables Proxier"
I1221 03:23:16.195144       1 proxier.go:242] "Setting route_localnet=1 to allow node-ports on localhost; to change this either disable iptables.localhostNodePorts (--iptables-localhost-nodeports) or set nodePortAddresses (--nodeport-addresses) to filter loopback addresses" ipFamily="IPv4"
I1221 03:23:16.226391       1 server.go:527] "Version info" version="v1.34.0"
I1221 03:23:16.226429       1 server.go:529] "Golang settings" GOGC="" GOMAXPROCS="" GOTRACEBACK=""
I1221 03:23:16.227756       1 config.go:200] "Starting service config controller"
I1221 03:23:16.227767       1 config.go:403] "Starting serviceCIDR config controller"
I1221 03:23:16.227781       1 shared_informer.go:349] "Waiting for caches to sync" controller="service config"
I1221 03:23:16.227784       1 shared_informer.go:349] "Waiting for caches to sync" controller="serviceCIDR config"
I1221 03:23:16.227782       1 config.go:106] "Starting endpoint slice config controller"
I1221 03:23:16.227795       1 shared_informer.go:349] "Waiting for caches to sync" controller="endpoint slice config"
I1221 03:23:16.227885       1 config.go:309] "Starting node config controller"
I1221 03:23:16.227931       1 shared_informer.go:349] "Waiting for caches to sync" controller="node config"
I1221 03:23:16.227941       1 shared_informer.go:356] "Caches are synced" controller="node config"
I1221 03:23:16.328851       1 shared_informer.go:356] "Caches are synced" controller="service config"
I1221 03:23:16.328956       1 shared_informer.go:356] "Caches are synced" controller="endpoint slice config"
I1221 03:23:16.329088       1 shared_informer.go:356] "Caches are synced" controller="serviceCIDR config"
```

6. How does this Kubernetes cluster ensure that a kube-proxy pod runs on all nodes in the cluster?

Inspect the kube-proxy pods and try to identify how they are deployed.

```
kubectl get ds -n kube-system
```

# Core-Dns-Lab

1. Identify the DNS solution implemented in this cluster.

```
Run the command: kubectl get pods -n kube-system and look for the DNS pods.
```

2. What is the name of the service created for accessing CoreDNS?

```
Run the command: kubectl get service -n kube-system
```

3. What is the IP of the CoreDNS server that should be configured on PODs to resolve services?

```
Run the command: kubectl get service -n kube-system and look for cluster IP value.

controlplane ~ ➜  kubectl get service -n kube-system
NAME       TYPE        CLUSTER-IP    EXTERNAL-IP   PORT(S)                  AGE
kube-dns   ClusterIP   172.20.0.10   <none>        53/UDP,53/TCP,9153/TCP   23m

controlplane ~ ➜  k create deploy ngin --image=nginx
deployment.apps/ngin created

controlplane ~ ➜  k get pods -o wide
NAME                    READY   STATUS    RESTARTS   AGE     IP           NODE           NOMINATED NODE   READINESS GATES
hr                      1/1     Running   0          9m35s   172.17.0.5   controlplane   <none>           <none>
ngin-6dc76f44c4-rfxcx   1/1     Running   0          4s      172.17.0.9   controlplane   <none>           <none>
simple-webapp-1         1/1     Running   0          9m22s   172.17.0.7   controlplane   <none>           <none>
simple-webapp-122       1/1     Running   0          9m22s   172.17.0.8   controlplane   <none>           <none>
test                    1/1     Running   0          9m35s   172.17.0.6   controlplane   <none>           <none>

```

4. Where is the configuration file located for configuring the CoreDNS service?

```
Inspect the Args field of the coredns deployment and check the file used.

controlplane ~ ➜  k describe deploy coredns -n kube-system
Name:                   coredns
Namespace:              kube-system
CreationTimestamp:      Sun, 21 Dec 2025 05:14:41 +0000
Labels:                 k8s-app=kube-dns
Annotations:            deployment.kubernetes.io/revision: 1
Selector:               k8s-app=kube-dns
Replicas:               2 desired | 2 updated | 2 total | 2 available | 0 unavailable
StrategyType:           RollingUpdate
MinReadySeconds:        0
RollingUpdateStrategy:  1 max unavailable, 25% max surge
Pod Template:
  Labels:           k8s-app=kube-dns
  Service Account:  coredns
  Containers:
   coredns:
    Image:       registry.k8s.io/coredns/coredns:v1.10.1
    Ports:       53/UDP (dns), 53/TCP (dns-tcp), 9153/TCP (metrics), 8080/TCP (liveness-probe), 8181/TCP (readiness-probe)
    Host Ports:  0/UDP (dns), 0/TCP (dns-tcp), 0/TCP (metrics), 0/TCP (liveness-probe), 0/TCP (readiness-probe)
    Args:
      -conf
      /etc/coredns/Corefile

```
5. How is the Corefile passed into the CoreDNS POD?
   
```
volumeMounts:
        - mountPath: /etc/coredns
          name: config-volume
          readOnly: true

volumes:
      - configMap:
          defaultMode: 420
          items:
          - key: Corefile
            path: Corefile
          name: coredns
        name: config-volume
```
6. What is the root domain/zone configured for this kubernetes cluster?

```
Run the command: kubectl describe configmap coredns -n kube-system and look for the entry after kubernetes.

controlplane ~ ➜  kubectl describe configmap coredns -n kube-system
Name:         coredns
Namespace:    kube-system
Labels:       <none>
Annotations:  <none>

Data
====
Corefile:
----
.:53 {
    errors
    health {
       lameduck 5s
    }
    ready
    kubernetes cluster.local in-addr.arpa ip6.arpa {
       pods insecure
       fallthrough in-addr.arpa ip6.arpa
       ttl 30
    }
    prometheus :9153
    forward . /etc/resolv.conf {
       max_concurrent 1000
    }
    cache 30
    loop
    reload
    loadbalance
}



BinaryData
====

Events:  <none>
```

7. Test pod exists in default ns and web pod exists in payroll ns. now test needs to access payroll, so what is the svc name

```
web-service.payroll

controlplane ~ ➜  k describe svc web-service -n payroll
Name:                     web-service
Namespace:                payroll
Labels:                   <none>
Annotations:              <none>
Selector:                 name=web
Type:                     ClusterIP
IP Family Policy:         SingleStack
IP Families:              IPv4
IP:                       172.20.101.193
IPs:                      172.20.101.193
Port:                     <unset>  80/TCP
TargetPort:               80/TCP
Endpoints:                172.17.0.4:80
Session Affinity:         None
Internal Traffic Policy:  Cluster
Events:                   <none>
```

## Ingress-lab1

1. Which namespace is the Ingress Resource deployed in?

```
controlplane ~ ➜  k get ingress -A
NAMESPACE   NAME                 CLASS    HOSTS   ADDRESS        PORTS   AGE
app-space   ingress-wear-watch   <none>   *       172.20.47.69   80      7m37s
```

2. What is the Host configured on the Ingress Resource?

The host entry defines the domain name that users use to reach the application like www.google.com

```
controlplane ~ ➜  k get ingress -A
NAMESPACE   NAME                 CLASS    HOSTS   ADDRESS        PORTS   AGE
app-space   ingress-wear-watch   <none>   *       172.20.47.69   80      7m37s

it set to * , so all hosts are applicable
```

3. What backend is the /wear path on the Ingress configured with?

```
Run the command: kubectl describe ingress --namespace app-space and look under the Rules section.

controlplane ~ ➜  k describe ingress ingress-wear-watch -n app-space 
Name:             ingress-wear-watch
Labels:           <none>
Namespace:        app-space
Address:          172.20.47.69
Ingress Class:    <none>
Default backend:  <default>
Rules:
  Host        Path  Backends
  ----        ----  --------
  *           
              /wear    wear-service:8080 (172.17.0.4:8080)
              /watch   video-service:8080 (172.17.0.5:8080)
Annotations:  nginx.ingress.kubernetes.io/rewrite-target: /
              nginx.ingress.kubernetes.io/ssl-redirect: false
Events:
  Type    Reason  Age                From                      Message
  ----    ------  ----               ----                      -------
  Normal  Sync    11m (x2 over 11m)  nginx-ingress-controller  Scheduled for sync

Answer: wear-service
```

4. At what path is the video streaming application made available on the Ingress?

```
Run the command: kubectl describe ingress --namespace app-space and look under the Rules section.

/watch 
```

5. If the requirement does not match any of the configured paths in the Ingress, to which service are the requests forwarded?

```
Execute the command kubectl describe ingress --namespace app-space and examine the Default backend field. If it displays <default>, proceed to inspect the ingress controller's manifest by executing kubectl get deploy ingress-nginx-controller -n ingress-nginx -o yaml. In the manifest, search for the argument --default-backend-service

controlplane ~ ➜  kubectl get deploy ingress-nginx-controller -n ingress-nginx -o yaml | grep default
        - --default-backend-service=app-space/default-backend-service
      schedulerName: default-scheduler
          defaultMode: 420

```

6. You are requested to change the URLs at which the applications are made available.

Make the video application available at /stream.

- Below is the ingress yaml
  
```yaml
apiVersion: networking.k8s.io/v1
kind: Ingress
metadata:
  annotations:
    nginx.ingress.kubernetes.io/rewrite-target: /
    nginx.ingress.kubernetes.io/ssl-redirect: "false"
  creationTimestamp: "2025-12-22T13:37:48Z"
  generation: 2
  name: ingress-wear-watch
  namespace: app-space
  resourceVersion: "3010"
  uid: 53be3562-c9a8-4b0f-995d-95c0eb5175fe
spec:
  rules:
  - http:
      paths:
      - backend:
          service:
            name: wear-service
            port:
              number: 8080
        path: /wear
        pathType: Prefix
      - backend:
          service:
            name: video-service
            port:
              number: 8080
        path: /stream
        pathType: Prefix
status:
  loadBalancer:
    ingress:
    - ip: 172.20.47.69

```

7. You are requested to add a new path to your ingress to make the food delivery application available to your customers.
   Make the new application available at /eat.
   
   ```
   Run the command: kubectl edit ingress --namespace app-space and add a new Path entry for the new service.
   ```
```
controlplane ~ ✖ k get svc -A
NAMESPACE       NAME                                 TYPE        CLUSTER-IP       EXTERNAL-IP   PORT(S)                      AGE
app-space       default-backend-service              ClusterIP   172.20.72.130    <none>        80/TCP                       35m
app-space       food-service                         ClusterIP   172.20.211.219   <none>        8080/TCP                     6m21s
app-space       video-service                        ClusterIP   172.20.80.238    <none>        8080/TCP                     35m

controlplane ~ ➜  k get pods -A | grep food
app-space       webapp-food-5b4489ff8-2fzjf                 1/1     Running     0          7m16s

```

   ```yaml

   Solution manifest file to add a new path to our ingress service to make the application available at /eat as follows:

apiVersion: networking.k8s.io/v1
kind: Ingress
metadata:
  annotations:
    nginx.ingress.kubernetes.io/rewrite-target: /
    nginx.ingress.kubernetes.io/ssl-redirect: "false"
  name: ingress-wear-watch
  namespace: app-space
spec:
  rules:
  - http:
      paths:
      - backend:
          service:
            name: wear-service
            port: 
              number: 8080
        path: /wear
        pathType: Prefix
      - backend:
          service:
            name: video-service
            port: 
              number: 8080
        path: /stream
        pathType: Prefix
      - backend:
          service:
            name: food-service
            port: 
              number: 8080
        path: /eat
        pathType: Prefix

   ```

8. You are requested to create the ingress name as critical-ingress to make the new application available at /pay.


Identify and implement the best approach to making this application available on the ingress controller and test to make sure its working.

Look into annotations: rewrite-target as well.

```
controlplane ~ ➜  k get all -n critical-space
NAME                              READY   STATUS    RESTARTS   AGE
pod/webapp-pay-85946fb65b-85dqp   1/1     Running   0          96s

NAME                  TYPE        CLUSTER-IP      EXTERNAL-IP   PORT(S)    AGE
service/pay-service   ClusterIP   172.20.28.100   <none>        8282/TCP   96s

NAME                         READY   UP-TO-DATE   AVAILABLE   AGE
deployment.apps/webapp-pay   1/1     1            1           96s

NAME                                    DESIRED   CURRENT   READY   AGE
replicaset.apps/webapp-pay-85946fb65b   1         1         1       96s

controlplane ~ ➜


hint:
Create a new Ingress for the new pay application in the critical-space namespace.

Use the command kubectl get svc -n critical-space to know the service and port details.
```

```yaml
---
apiVersion: networking.k8s.io/v1
kind: Ingress
metadata:
  name: critical-ingress
  namespace: critical-space
  annotations:
    nginx.ingress.kubernetes.io/rewrite-target: /
    nginx.ingress.kubernetes.io/ssl-redirect: "false"
spec:
  rules:
  - http:
      paths:
      - path: /pay
        pathType: Prefix
        backend:
          service:
           name: pay-service
           port:
            number: 8282
```

## Ingress-lab2

1. Let us now deploy an Ingress Controller. First, create a namespace called ingress-nginx.

```
kubectl create namespace ingress-nginx
```

2. The NGINX Ingress Controller requires a ConfigMap object. Create a ConfigMap object with name ingress-nginx-controller in the ingress-nginx namespace.

No data needs to be configured in the ConfigMap.

```
kubectl create configmap ingress-nginx-controller --namespace ingress-nginx
```

3. The NGINX Ingress Controller requires two ServiceAccounts. Create both ServiceAccount with name ingress-nginx and ingress-nginx-admission in the ingress-nginx namespace.


Use the spec provided below.

```
kubectl create serviceaccount ingress-nginx --namespace ingress-nginx and
kubectl create serviceaccount ingress-nginx-admission --namespace ingress-nginx
```

4. We just created role and rolebindings, just inspect that

```
controlplane ~ ➜  k get roles -n ingress-nginx
NAME                      CREATED AT
ingress-nginx             2025-12-23T13:54:20Z
ingress-nginx-admission   2025-12-23T13:54:20Z

controlplane ~ ➜  k get rolebindings -n ingress-nginx
NAME                      ROLE                           AGE
ingress-nginx             Role/ingress-nginx             4m6s
ingress-nginx-admission   Role/ingress-nginx-admission   4m6s


controlplane ~ ➜  k describe role ingress-nginx-admission -n ingress-nginx
Name:         ingress-nginx-admission
Labels:       app.kubernetes.io/component=admission-webhook
              app.kubernetes.io/instance=ingress-nginx
              app.kubernetes.io/managed-by=Helm
              app.kubernetes.io/name=ingress-nginx
              app.kubernetes.io/part-of=ingress-nginx
              app.kubernetes.io/version=1.1.2
              helm.sh/chart=ingress-nginx-4.0.18
Annotations:  helm.sh/hook: pre-install,pre-upgrade,post-install,post-upgrade
              helm.sh/hook-delete-policy: before-hook-creation,hook-succeeded
PolicyRule:
  Resources  Non-Resource URLs  Resource Names  Verbs
  ---------  -----------------  --------------  -----
  secrets    []                 []              [get create]

```

5. Let us now deploy the Ingress Controller. Create the Kubernetes objects using the given file.


The Deployment and it's service configuration is given at /root/ingress-controller.yaml. There are several issues with it. Try to fix them.

Note: Do not edit the default image provided in the given file. The image validation check passes when other issues are resolved.

Note: After the ingress controller is running, click the Ingress button at the top right to view the ingress. The page shows a 404 error, it is because the ingress resource has not been created.

- Below is the ingress controller yaml
  
```yaml

apiVersion: apps/v1
kind: Deployment
metadata:
  labels:
    app.kubernetes.io/component: controller
    app.kubernetes.io/instance: ingress-nginx
    app.kubernetes.io/managed-by: Helm
    app.kubernetes.io/name: ingress-nginx
    app.kubernetes.io/part-of: ingress-nginx
    app.kubernetes.io/version: 1.1.2
    helm.sh/chart: ingress-nginx-4.0.18
  name: ingress-nginx-controller
  namespace: ingress-nginx
spec:
  minReadySeconds: 0
  revisionHistoryLimit: 10
  selector:
    matchLabels:
      app.kubernetes.io/component: controller
      app.kubernetes.io/instance: ingress-nginx
      app.kubernetes.io/name: ingress-nginx
  template:
    metadata:
      labels:
        app.kubernetes.io/component: controller
        app.kubernetes.io/instance: ingress-nginx
        app.kubernetes.io/name: ingress-nginx
    spec:
      containers:
      - args:
        - /nginx-ingress-controller
        - --publish-service=$(POD_NAMESPACE)/ingress-nginx-controller
        - --election-id=ingress-controller-leader
        - --watch-ingress-without-class=true
        - --default-backend-service=app-space/default-http-backend
        - --controller-class=k8s.io/ingress-nginx
        - --ingress-class=nginx
        - --configmap=$(POD_NAMESPACE)/ingress-nginx-controller
        - --validating-webhook=:8443
        - --validating-webhook-certificate=/usr/local/certificates/cert
        - --validating-webhook-key=/usr/local/certificates/key
        env:
        - name: POD_NAME
          valueFrom:
            fieldRef:
              fieldPath: metadata.name
        - name: POD_NAMESPACE
          valueFrom:
            fieldRef:
              fieldPath: metadata.namespace
        - name: LD_PRELOAD
          value: /usr/local/lib/libmimalloc.so
        image: registry.k8s.io/ingress-nginx/controller:v1.1.2@sha256:28b11ce69e57843de44e3db6413e98d09de0f6688e33d4bd384002a44f78405c
        imagePullPolicy: IfNotPresent
        lifecycle:
          preStop:
            exec:
              command:
              - /wait-shutdown
        livenessProbe:
          failureThreshold: 5
          httpGet:
            path: /healthz
            port: 10254
            scheme: HTTP
          initialDelaySeconds: 10
          periodSeconds: 10
          successThreshold: 1
          timeoutSeconds: 1
        name: controller
        ports:
        - name: http
          containerPort: 80
          protocol: TCP
        - containerPort: 443
          name: https
          protocol: TCP
        - containerPort: 8443
          name: webhook
          protocol: TCP
        readinessProbe:
          failureThreshold: 3
          httpGet:
            path: /healthz
            port: 10254
            scheme: HTTP
          initialDelaySeconds: 10
          periodSeconds: 10
          successThreshold: 1
          timeoutSeconds: 1
        resources:
          requests:
            cpu: 100m
            memory: 90Mi
        securityContext:
          allowPrivilegeEscalation: true
          capabilities:
            add:
            - NET_BIND_SERVICE
            drop:
            - ALL
          runAsUser: 101
        volumeMounts:
        - mountPath: /usr/local/certificates/
          name: webhook-cert
          readOnly: true
      dnsPolicy: ClusterFirst
      nodeSelector:
        kubernetes.io/os: linux
      serviceAccountName: ingress-nginx
      terminationGracePeriodSeconds: 300
      volumes:
      - name: webhook-cert
        secret:
          secretName: ingress-nginx-admission

---

apiVersion: v1
kind: Service
metadata:
  creationTimestamp: null
  labels:
    app.kubernetes.io/component: controller
    app.kubernetes.io/instance: ingress-nginx
    app.kubernetes.io/managed-by: Helm
    app.kubernetes.io/name: ingress-nginx
    app.kubernetes.io/part-of: ingress-nginx
    app.kubernetes.io/version: 1.1.2
    helm.sh/chart: ingress-nginx-4.0.18
  name: ingress-nginx-controller
  namespace: ingress-nginx
spec:
  ports:
  - port: 80
    protocol: TCP
    targetPort: 80
    nodePort: 30080
  selector:
    app.kubernetes.io/component: controller
    app.kubernetes.io/instance: ingress-nginx
    app.kubernetes.io/name: ingress-nginx
  type: NodePort

```

- Another yaml

```
k expose deploy <deploy-name> -n <namespace> --name <service-name> --port=80 --target-port=80 --type NodePort
```

6. Create the ingress resource to make the applications available at /wear and /watch on the Ingress service.

Also, make use of rewrite-target annotation field: -

nginx.ingress.kubernetes.io/rewrite-target: /


Ingress resource comes under the namespace scoped, so don't forget to create the ingress in the app-space namespace.

```yaml
---
apiVersion: networking.k8s.io/v1
kind: Ingress
metadata:
  name: ingress-wear-watch
  namespace: app-space
  annotations:
    nginx.ingress.kubernetes.io/rewrite-target: /
    nginx.ingress.kubernetes.io/ssl-redirect: "false"
spec:
  rules:
  - http:
      paths:
      - path: /wear
        pathType: Prefix
        backend:
          service:
           name: wear-service
           port: 
            number: 8080
      - path: /watch
        pathType: Prefix
        backend:
          service:
           name: video-service
           port:
            number: 8080
```

## gateway-api-lab

1. Which API resource is used to define a Gateway in Kubernetes?

```
The Gateway resource is used to define an instance of a Gateway API implementation in Kubernetes.
It acts as an entry point for external traffic into the cluster.

The GatewayClass resource defines a type of Gateway (i.e., the controller and its capabilities),
but does not create an actual Gateway instance.

To actually deploy a Gateway that routes traffic, you must create a Gateway resource.
```

2. What is the purpose of the allowedRoutes field in a Gateway?

The allowedRoutes field in a Gateway listener determines which namespaces and types of Routes (like HTTPRoute, TCPRoute, etc.) are allowed to attach to that Gateway.

By default, only Routes in the same namespace as the Gateway can attach.
Setting namespaces.from: All allows Routes from all namespaces to attach.
You can also restrict which kinds of Routes can attach using the kinds field.
This enables fine-grained access control and safe multi-tenant routing.
Example:
```
spec:
  listeners:
    - name: http
      port: 80
      protocol: HTTP
      allowedRoutes:
        namespaces:
          from: All  # Allows Routes from all namespaces to attach
        kinds:
          - group: gateway.networking.k8s.io
            kind: HTTPRoute
```

3. Which of the following protocols is NOT supported by the Kubernetes Gateway API?

The Kubernetes Gateway API supports HTTP, HTTPS, TCP, UDP, and TLS protocols.
However, ICMP is not supported, as it is not a transport-layer protocol used for application traffic.

4. How does a GatewayClass differ from a Gateway?

A GatewayClass is similar to an IngressClass; it defines the controller-specific behavior of a Gateway.
A Gateway is an instance that references a GatewayClass to determine its underlying implementation.

In short:

GatewayClass: The template or type.
Gateway: The deployed instance that uses the template.

5. What is the primary advantage of using Gateway API over Ingress?

The Gateway API provides more advanced routing capabilities than Ingress, including:

Multi-protocol support (HTTP, TCP, UDP, etc.)
Better extensibility with Routes and Filters
More granular access control using AllowedRoutes
Unlike Ingress, which is primarily HTTP-based, Gateway API is designed for flexibility across multiple protocols.

6. To use the Gateway API, a controller is required. In this lab, we will install NGINX Gateway Fabric as the controller. Follow these steps to complete the installation:

1. Install the Gateway API resources
```
kubectl kustomize "https://github.com/nginx/nginx-gateway-fabric/config/crd/gateway-api/standard?ref=v1.5.1" | kubectl apply -f -
```
2. Deploy the NGINX Gateway Fabric CRDs
```
kubectl apply -f https://raw.githubusercontent.com/nginx/nginx-gateway-fabric/v1.6.1/deploy/crds.yaml
```
3. Deploy NGINX Gateway Fabric

```
kubectl apply -f https://raw.githubusercontent.com/nginx/nginx-gateway-fabric/v1.6.1/deploy/nodeport/deploy.yaml
```
4. Verify the Deployment

```
kubectl get pods -n nginx-gateway
```

5. View the nginx-gateway service

```
kubectl get svc -n nginx-gateway nginx-gateway -o yaml
```

6. Update the nginx-gateway service to expose ports 30080 for HTTP and 30081 for HTTPS

```
kubectl patch svc nginx-gateway -n nginx-gateway --type='json' -p='[
  {"op": "replace", "path": "/spec/ports/0/nodePort", "value": 30080},
  {"op": "replace", "path": "/spec/ports/1/nodePort", "value": 30081}
]'
```

7. Create a Kubernetes Gateway resource with the following specifications:

Name: nginx-gateway
Namespace: nginx-gateway
Gateway Class Name: nginx
Listeners:
Protocol: HTTP
Port: 80
Name: http
Allowed Routes: All namespaces

Prerequisite: Make sure the Gateway API CRDs are installed and a compatible controller (like NGINX Gateway Fabric) is running.

1. Create the Gateway manifest (gateway.yaml):

```yaml
apiVersion: gateway.networking.k8s.io/v1
kind: Gateway
metadata:
  name: nginx-gateway
  namespace: nginx-gateway
spec:
  gatewayClassName: nginx
  listeners:
    - name: http
      port: 80
      protocol: HTTP
      allowedRoutes: 
        namespaces: 
          from: All
```

2. Deploy the manifest:
```
kubectl apply -f gateway.yaml
```

3. To verify the succesfull deployment, run the commands below:

```
kubectl get gateways -n nginx-gateway
kubectl describe gateway nginx-gateway -n nginx-gateway
```

8. A new pod named frontend-app and a service called frontend-svc have been deployed in the default namespace.
Expose the service on the / path by creating an HTTPRoute named frontend-route.

```
kubectl get pod,svc -n default
```
1. Confirm the pod and service exist:

```
kubectl get pod,svc -n default
```

2. Create the HTTPRoute manifest:

Create a file named frontend-route.yaml with the following content:
```
apiVersion: gateway.networking.k8s.io/v1
kind: HTTPRoute
metadata:
  name: frontend-route
  namespace: default
spec:
  parentRefs:
    - name: nginx-gateway           # Name of the Gateway
      namespace: nginx-gateway      # Namespace where the Gateway is deployed
      sectionName: http             # Attach to the 'http' listener
  rules:
    - matches:
        - path:
            type: PathPrefix
            value: /
      backendRefs:
        - name: frontend-svc
          port: 80
```

- parentRefs attaches the route to the nginx-gateway Gateway in the nginx-gateway namespace, specifically to the http listener.
- The rule matches all requests with a path prefix of / and forwards them to the frontend-svc service on port 80.

3. Apply the manifest:

```
kubectl apply -f frontend-route.yaml
```

4. Verify the HTTPRoute:

```
kubectl get httproute frontend-route 
kubectl describe httproute frontend-route 
```

---

# Cluster-Installation

1. Prepare both nodes (controlplane and node01) for Kubernetes by completing the following steps:

Apply the necessary sysctl parameters for networking.
Install the kubeadm and kubelet packages at the exact version 1.34.0-1.1 on both nodes.
Install the kubectl package at the exact version 1.34.0-1.1 exclusively on the controlplane.

Refer to the official k8s documentation -
https://kubernetes.io/docs/setup/production-environment/tools/kubeadm/install-kubeadm/

These steps have to be performed on both nodes.

Enable IPv4 packet forwarding:

```
cat <<EOF | sudo tee /etc/sysctl.d/k8s.conf
net.ipv4.ip_forward=1
EOF

#Apply sysctl params without reboot

sudo sysctl --system

#verify that net.ipv4.ip_forward is set to 1 with:
sysctl net.ipv4.ip_forward
```

The container runtime has already been installed on both nodes, so you may skip this step.
Install kubeadm, kubectl and kubelet on all nodes:

```
sudo apt-get update

sudo apt-get install -y apt-transport-https ca-certificates curl

curl -fsSL https://pkgs.k8s.io/core:/stable:/v1.34/deb/Release.key | sudo gpg --dearmor -o /etc/apt/keyrings/kubernetes-apt-keyring.gpg

echo 'deb [signed-by=/etc/apt/keyrings/kubernetes-apt-keyring.gpg] https://pkgs.k8s.io/core:/stable:/v1.34/deb/ /' | sudo tee /etc/apt/sources.list.d/kubernetes.list

sudo apt-get update

# To see the new version labels
sudo apt-cache madison kubeadm

sudo apt-get install -y kubelet=1.34.0-1.1 kubeadm=1.34.0-1.1 kubectl=1.34.0-1.1

sudo apt-mark hold kubelet kubeadm kubectl
```

2. Initialize Control Plane Node (Master Node). Use the following options:


apiserver-advertise-address - Use the IP address allocated to eth0 on the controlplane node

apiserver-cert-extra-sans - Set it to controlplane

pod-network-cidr - Set to 172.17.0.0/16

service-cidr - Set to 172.20.0.0/16

Once done, set up the default kubeconfig file and wait for node to be part of the cluster.

**Hint**

Run
```
kubeadm init --apiserver-cert-extra-sans=controlplane --apiserver-advertise-address 192.168.57.62 --pod-network-cidr=172.17.0.0/16 --service-cidr=172.20.0.0/16
```

The IP address used here is just an example. It will change for your lab session. Make sure to check the IP address allocated to eth0 by running:

In this example, the IP address is 192.168.57.62

```
controlplane ~ ✖ ifconfig eth0
eth0: flags=4163<UP,BROADCAST,RUNNING,MULTICAST>  mtu 1410
        inet 192.168.57.62  netmask 255.255.255.255  broadcast 0.0.0.0
        inet6 fe80::801d:adff:fe75:697a  prefixlen 64  scopeid 0x20<link>
        ether 82:1d:ad:75:69:7a  txqueuelen 0  (Ethernet)
        RX packets 21630  bytes 150298755 (150.2 MB)
        RX errors 0  dropped 0  overruns 0  frame 0
        TX packets 19000  bytes 3580787 (3.5 MB)
        TX errors 0  dropped 1 overruns 0  carrier 0  collisions 0
```

**Solution**

You can use the below kubeadm init command to spin up the cluster:
```
IP_ADDR=$(ip addr show eth0 | grep -oP '(?<=inet\s)\d+(\.\d+){3}')
kubeadm init --apiserver-cert-extra-sans=controlplane --apiserver-advertise-address $IP_ADDR --pod-network-cidr=172.17.0.0/16 --service-cidr=172.20.0.0/16
```
Once you run the init command, you should see an output similar to below:

```
[init] Using Kubernetes version: v1.34.1
[preflight] Running pre-flight checks
        [WARNING SystemVerification]: cgroups v1 support is in maintenance mode, please migrate to cgroups v2
[preflight] Pulling images required for setting up a Kubernetes cluster
[preflight] This might take a minute or two, depending on the speed of your internet connection
[preflight] You can also perform this action beforehand using 'kubeadm config images pull'
W0520 03:05:34.890872   11137 checks.go:846] detected that the sandbox image "registry.k8s.io/pause:3.6" of the container runtime is inconsistent with that used by kubeadm.It is recommended to use "registry.k8s.io/pause:3.10" as the CRI sandbox image.
[certs] Using certificateDir folder "/etc/kubernetes/pki"
[certs] Generating "ca" certificate and key
[certs] Generating "apiserver" certificate and key
[certs] apiserver serving cert is signed for DNS names [controlplane kubernetes kubernetes.default kubernetes.default.svc kubernetes.default.svc.cluster.local] and IPs [172.20.0.1 192.168.9.95]
[certs] Generating "apiserver-kubelet-client" certificate and key
[certs] Generating "front-proxy-ca" certificate and key
[certs] Generating "front-proxy-client" certificate and key
[certs] Generating "etcd/ca" certificate and key
[certs] Generating "etcd/server" certificate and key
[certs] etcd/server serving cert is signed for DNS names [controlplane localhost] and IPs [192.168.9.95 127.0.0.1 ::1]
[certs] Generating "etcd/peer" certificate and key
[certs] etcd/peer serving cert is signed for DNS names [controlplane localhost] and IPs [192.168.9.95 127.0.0.1 ::1]
[certs] Generating "etcd/healthcheck-client" certificate and key
[certs] Generating "apiserver-etcd-client" certificate and key
[certs] Generating "sa" key and public key
[kubeconfig] Using kubeconfig folder "/etc/kubernetes"
[kubeconfig] Writing "admin.conf" kubeconfig file
[kubeconfig] Writing "super-admin.conf" kubeconfig file
[kubeconfig] Writing "kubelet.conf" kubeconfig file
[kubeconfig] Writing "controller-manager.conf" kubeconfig file
[kubeconfig] Writing "scheduler.conf" kubeconfig file
[etcd] Creating static Pod manifest for local etcd in "/etc/kubernetes/manifests"
[control-plane] Using manifest folder "/etc/kubernetes/manifests"
[control-plane] Creating static Pod manifest for "kube-apiserver"
[control-plane] Creating static Pod manifest for "kube-controller-manager"
[control-plane] Creating static Pod manifest for "kube-scheduler"
[kubelet-start] Writing kubelet environment file with flags to file "/var/lib/kubelet/kubeadm-flags.env"
[kubelet-start] Writing kubelet configuration to file "/var/lib/kubelet/config.yaml"
[kubelet-start] Starting the kubelet
[wait-control-plane] Waiting for the kubelet to boot up the control plane as static Pods from directory "/etc/kubernetes/manifests"
[kubelet-check] Waiting for a healthy kubelet at http://127.0.0.1:10248/healthz. This can take up to 4m0s
[kubelet-check] The kubelet is healthy after 1.001862161s
[control-plane-check] Waiting for healthy control plane components. This can take up to 4m0s
[control-plane-check] Checking kube-apiserver at https://192.168.9.95:6443/livez
[control-plane-check] Checking kube-controller-manager at https://127.0.0.1:10257/healthz
[control-plane-check] Checking kube-scheduler at https://127.0.0.1:10259/livez
[control-plane-check] kube-scheduler is healthy after 12.743187359s
[control-plane-check] kube-controller-manager is healthy after 17.259346714s
[control-plane-check] kube-apiserver is healthy after 35.501756821s
[upload-config] Storing the configuration used in ConfigMap "kubeadm-config" in the "kube-system" Namespace
[kubelet] Creating a ConfigMap "kubelet-config" in namespace kube-system with the configuration for the kubelets in the cluster
[upload-certs] Skipping phase. Please see --upload-certs
[mark-control-plane] Marking the node controlplane as control-plane by adding the labels: [node-role.kubernetes.io/control-plane node.kubernetes.io/exclude-from-external-load-balancers]
[mark-control-plane] Marking the node controlplane as control-plane by adding the taints [node-role.kubernetes.io/control-plane:NoSchedule]
[bootstrap-token] Using token: 50bd8b.x003t0tq1fzmultg
[bootstrap-token] Configuring bootstrap tokens, cluster-info ConfigMap, RBAC Roles
[bootstrap-token] Configured RBAC rules to allow Node Bootstrap tokens to get nodes
[bootstrap-token] Configured RBAC rules to allow Node Bootstrap tokens to post CSRs in order for nodes to get long term certificate credentials
[bootstrap-token] Configured RBAC rules to allow the csrapprover controller automatically approve CSRs from a Node Bootstrap Token
[bootstrap-token] Configured RBAC rules to allow certificate rotation for all node client certificates in the cluster
[bootstrap-token] Creating the "cluster-info" ConfigMap in the "kube-public" namespace
[kubelet-finalize] Updating "/etc/kubernetes/kubelet.conf" to point to a rotatable kubelet client certificate and key
[addons] Applied essential addon: CoreDNS
[addons] Applied essential addon: kube-proxy

Your Kubernetes control-plane has initialized successfully!

To start using your cluster, you need to run the following as a regular user:

  mkdir -p $HOME/.kube
  sudo cp -i /etc/kubernetes/admin.conf $HOME/.kube/config
  sudo chown $(id -u):$(id -g) $HOME/.kube/config

Alternatively, if you are the root user, you can run:

  export KUBECONFIG=/etc/kubernetes/admin.conf

You should now deploy a pod network to the cluster.
Run "kubectl apply -f [podnetwork].yaml" with one of the options listed at:
  https://kubernetes.io/docs/concepts/cluster-administration/addons/

Then you can join any number of worker nodes by running the following on each as root:

kubeadm join 192.168.9.95:6443 --token 50bd8b.x003t0tq1fzmultg \
        --discovery-token-ca-cert-hash sha256:70a59cdbfc5f5a3f0d49ca90198525870da1fe6f02def53d80a2c00fcc4bde72
```

Once the command has been run successfully, set up the kubeconfig:

```
mkdir -p $HOME/.kube

sudo cp -i /etc/kubernetes/admin.conf $HOME/.kube/config

sudo chown $(id -u):$(id -g) $HOME/.kube/config
```

3. Generate a kubeadm join token

Or copy the one that was generated by kubeadm init command and join node01

```
kubeadm join 192.168.57.62:6443 --token aueub0.uhnoxmmksnoho94g \
        --discovery-token-ca-cert-hash sha256:4cf1b46be87082562bd3404db607c0c77ecfd6722870acfaae739be67a967426
```
```
node01 ~ ➜  kubeadm join 192.168.57.62:6443 --token aueub0.uhnoxmmksnoho94g \
        --discovery-token-ca-cert-hash sha256:4cf1b46be87082562bd3404db607c0c77ecfd6722870acfaae739be67a967426
[preflight] Running pre-flight checks
        [WARNING SystemVerification]: cgroups v1 support is in maintenance mode, please migrate to cgroups v2
[preflight] Reading configuration from the "kubeadm-config" ConfigMap in namespace "kube-system"...
[preflight] Use 'kubeadm init phase upload-config kubeadm --config your-config-file' to re-upload it.
[kubelet-start] Writing kubelet configuration to file "/var/lib/kubelet/instance-config.yaml"
[patches] Applied patch of type "application/strategic-merge-patch+json" to target "kubeletconfiguration"
[kubelet-start] Writing kubelet configuration to file "/var/lib/kubelet/config.yaml"
[kubelet-start] Writing kubelet environment file with flags to file "/var/lib/kubelet/kubeadm-flags.env"
[kubelet-start] Starting the kubelet
[kubelet-check] Waiting for a healthy kubelet at http://127.0.0.1:10248/healthz. This can take up to 4m0s
[kubelet-check] The kubelet is healthy after 1.501798854s
[kubelet-start] Waiting for the kubelet to perform the TLS Bootstrap

This node has joined the cluster:
* Certificate signing request was sent to apiserver and a response was received.
* The Kubelet was informed of the new secure connection details.

Run 'kubectl get nodes' on the control-plane to see this node join the cluster.

```
```
controlplane ~ ➜  k get nodes
NAME           STATUS     ROLES           AGE     VERSION
controlplane   NotReady   control-plane   5m21s   v1.34.0
node01         NotReady   <none>          7s      v1.34.0
```

4. To install a network plugin, we will go with Flannel as the default choice. For inter-host communication, we will utilize the eth0 interface and update the Network field accordingly.

Ensure that the Flannel manifest includes the appropriate options for this configuration.


For detailed instructions, refer to the official documentation linked in the upper right corner above the terminal.

**Hint**
Install flannel CNI and make sure to specify the interface to eth0 and update the Network field.

**Solution**

On the controlplane node, run the following set of commands to deploy the network plugin:

Download the original YAML file and save it as kube-flannel.yml:
```
curl -LO https://raw.githubusercontent.com/flannel-io/flannel/v0.20.2/Documentation/kube-flannel.yml
```

2. Open the kube-flannel.yml file using a text editor.

3. We are using a custom PodCIDR (172.17.0.0/16) instead of the default 10.244.0.0/16 when bootstrapping the Kubernetes cluster. However, the Flannel manifest by default is configured to use 10.244.0.0/16 as its network, which does not align with the specified PodCIDR. To resolve this, we need to update the Network field in the kube-flannel-cfg ConfigMap to match the custom PodCIDR defined during cluster initialization.

```
172.17.0.0/16

net-conf.json: |
    {
      "Network": "172.17.0.0/16", # Update this to match the custom PodCIDR
      "Backend": {
        "Type": "vxlan"
      }
```

4. Locate the args section within the kube-flannel container definition. It should look like this:

```
  args:
  - --ip-masq
  - --kube-subnet-mgr
```
5. Add the additional argument - --iface=eth0 to the existing list of arguments.

```
kubectl apply -f kube-flannel.yml
```
6. Now apply the modified manifest kube-flannel.yml file using kubectl:

```
kubectl apply -f kube-flannel.yml
```

After applying the manifest, wait for all the pods to become in the Ready state. You can use the watch command to monitor the pod status:

```
watch kubectl get pods -A
```

Example of expected pods:

```
controlplane ~ ➜  kubectl get pods -A
NAMESPACE      NAME                                   READY   STATUS    RESTARTS   AGE
kube-flannel   kube-flannel-ds-gc5kf                  1/1     Running   0          54s
kube-flannel   kube-flannel-ds-mtjd6                  1/1     Running   0          54s
kube-system    coredns-668d6bf9bc-7lf7s               1/1     Running   0          3m31s
kube-system    coredns-668d6bf9bc-jl8t6               1/1     Running   0          3m31s
kube-system    etcd-controlplane                      1/1     Running   0          3m37s
kube-system    kube-apiserver-controlplane            1/1     Running   0          3m37s
kube-system    kube-controller-manager-controlplane   1/1     Running   0          3m37s
kube-system    kube-proxy-t5wrt                       1/1     Running   0          3m31s
kube-system    kube-proxy-trmhs                       1/1     Running   0          3m8s
kube-system    kube-scheduler-controlplane            1/1     Running   0          3m37s
```

After all the pods are in the Ready state, the status of both nodes should now become Ready:

```
controlplane ~ ➜  k get nodes
NAME           STATUS   ROLES           AGE   VERSION
controlplane   Ready    control-plane   16m   v1.34.0
node01         Ready    <none>          10m   v1.34.0
```

## Install helm

1. Install the helm by using below doc

https://helm.sh/docs/intro/install/#from-apt-debianubuntu

2. identify version

```
helm version
```

3. Which environment variable can be used to indicate whether or not Helm is running in Debug mode?

Use the help mode of the helm command to look for this information.

use below help command and check
```
helm -h
```
