## Notes

https://notes.kodekloud.com/docs/CKA-Certification-Course-Certified-Kubernetes-Administrator/Introduction/Course-Introduction

Git: https://github.com/SeshadriRC/certified-kubernetes-administrator-course

# Service

**NodePort**
- External traffic hits the node IP → NodePort → Service → Pods.


- [Networking](#networking)
  - [DNS](#DNS)
  - [Network-namespace](#network-namespace)


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
## Lab

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

---
