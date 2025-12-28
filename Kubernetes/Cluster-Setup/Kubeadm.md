**Step 1:**

Total 5 steps given , perform all that
**Install kubeadm on all nodes - controlplane,node01 and node02**
https://kubernetes.io/docs/setup/production-environment/tools/kubeadm/install-kubeadm/#installing-kubeadm-kubelet-and-kubectl

Check kubeadm version

```
kubeadm version
```

**Step 2:**

https://kubernetes.io/docs/setup/production-environment/container-runtimes/

Install container run time on all master and worker nodes, here we are installing containerd

```
sudo apt update
sudo apt install -y containerd
```

- Now install systemd cgroupfs drive, before that check what our linux uses.
- so below one uses systemd, so install systemd cgroup driver
```
vagrant@node02:~$ ps -p 1
    PID TTY          TIME CMD
      1 ?        00:00:06 systemd

```

- As per the below doc if we are using kubernetes version > 1.21 then automatically system, user does not set the cgroupDriver field under KubeletConfiguration

https://kubernetes.io/docs/tasks/administer-cluster/kubeadm/configure-cgroup-driver/#configuring-the-kubelet-cgroup-driver

- However we need to configure systemd cgroup driver by following the below link

https://kubernetes.io/docs/setup/production-environment/container-runtimes/#containerd-systemd

below things we need to modify as per doc
```
[plugins."io.containerd.grpc.v1.cri".containerd.runtimes.runc]
  ...
  [plugins."io.containerd.grpc.v1.cri".containerd.runtimes.runc.options]
    SystemdCgroup = true
```

- create directory

```
sudo mkdir -p /etc/containerd
```


To check the default containerd configurations
```
containerd config default
```

```
containerd config default | sed 's/SystemdCgroup = false/SystemdCgroup = true/' | sudo tee /etc/containerd/config.toml
```

- Verify

```
cat /etc/containerd/config.toml | grep -i SystemdCgroup -B 50
```

- Restart containerd

```
sudo systemctl restart containerd
```

**Step 5: Enable IP forwarding**

i got below error, so following this step
```
vagrant@controlplane:~$ sudo kubeadm init --apiserver-advertise-address 192.168.1.7 --pod-network-cidr "10.244.0.0/16" --upload-certs
[init] Using Kubernetes version: v1.35.0
[preflight] Running pre-flight checks
        [WARNING ContainerRuntimeVersion]: You must update your container runtime to a version that supports the CRI method RuntimeConfig. Falling back to using cgroupDriver from kubelet config will be removed in 1.36. For more information, see https://git.k8s.io/enhancements/keps/sig-node/4033-group-driver-detection-over-cri
[preflight] Some fatal errors occurred:
        [ERROR FileContent--proc-sys-net-ipv4-ip_forward]: /proc/sys/net/ipv4/ip_forward contents are not set to 1
[preflight] If you know what you are doing, you can make a check non-fatal with `--ignore-preflight-errors=...`
error: error execution phase preflight: preflight checks failed
To see the stack trace of this error execute with --v=5 or higher
vagrant@controlplane:~$ cat /proc/sys/net/ipv4/ip_forward
0
```

- Linux is currently not allowing packets to be forwarded

- Kubernetes networking (Pods ↔ Nodes ↔ Services) will not work without this

- Hence kubeadm stops during preflight checks

```
[ERROR FileContent--proc-sys-net-ipv4-ip_forward]:
/proc/sys/net/ipv4/ip_forward contents are not set to 1
```

- Now we need to set it on all controlplane and master nodes

**Temporary fix**
```
sudo sysctl -w net.ipv4.ip_forward=1
sudo cat /proc/sys/net/ipv4/ip_forward
```

**Permanent fix**
```
sudo tee /etc/sysctl.d/k8s.conf <<EOF
net.ipv4.ip_forward = 1
EOF

sudo sysctl --system

sysctl net.ipv4.ip_forward
```

**Step 4: Initialize the controlplane node**


https://kubernetes.io/docs/setup/production-environment/tools/kubeadm/create-cluster-kubeadm/#considerations-about-apiserver-advertise-address-and-controlplaneendpoint

```
sudo kubeadm init --apiserver-advertise-address <master-node-ip> --pod-network-cidr "10.244.0.0/16" --upload-certs

Eg: sudo kubeadm init --apiserver-advertise-address 192.168.1.7 --pod-network-cidr "10.244.0.0/16" --upload-certs
```

**output**

```
vagrant@controlplane:~$ sudo kubeadm init --apiserver-advertise-address 192.168.1.7 --pod-network-cidr "10.244.0.0/16" --upload-certs
[init] Using Kubernetes version: v1.35.0
[preflight] Running pre-flight checks
        [WARNING ContainerRuntimeVersion]: You must update your container runtime to a version that supports the CRI method RuntimeConfig. Falling back to using cgroupDriver from kubelet config will be removed in 1.36. For more information, see https://git.k8s.io/enhancements/keps/sig-node/4033-group-driver-detection-over-cri
[preflight] Pulling images required for setting up a Kubernetes cluster
[preflight] This might take a minute or two, depending on the speed of your internet connection
[preflight] You can also perform this action beforehand using 'kubeadm config images pull'
W1228 15:43:08.429819    6725 checks.go:906] detected that the sandbox image "registry.k8s.io/pause:3.8" of the container runtime is inconsistent with that used by kubeadm. It is recommended to use "registry.k8s.io/pause:3.10.1" as the CRI sandbox image.
[certs] Using certificateDir folder "/etc/kubernetes/pki"
[certs] Generating "ca" certificate and key
[certs] Generating "apiserver" certificate and key
[certs] apiserver serving cert is signed for DNS names [controlplane kubernetes kubernetes.default kubernetes.default.svc kubernetes.default.svc.cluster.local] and IPs [10.96.0.1 192.168.1.7]
[certs] Generating "apiserver-kubelet-client" certificate and key
[certs] Generating "front-proxy-ca" certificate and key
[certs] Generating "front-proxy-client" certificate and key
[certs] Generating "etcd/ca" certificate and key
[certs] Generating "etcd/server" certificate and key
[certs] etcd/server serving cert is signed for DNS names [controlplane localhost] and IPs [192.168.1.7 127.0.0.1 ::1]
[certs] Generating "etcd/peer" certificate and key
[certs] etcd/peer serving cert is signed for DNS names [controlplane localhost] and IPs [192.168.1.7 127.0.0.1 ::1]
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
[kubelet-start] Writing kubelet configuration to file "/var/lib/kubelet/instance-config.yaml"
[patches] Applied patch of type "application/strategic-merge-patch+json" to target "kubeletconfiguration"
[kubelet-start] Writing kubelet configuration to file "/var/lib/kubelet/config.yaml"
[kubelet-start] Starting the kubelet
[wait-control-plane] Waiting for the kubelet to boot up the control plane as static Pods from directory "/etc/kubernetes/manifests"
[kubelet-check] Waiting for a healthy kubelet at http://127.0.0.1:10248/healthz. This can take up to 4m0s
[kubelet-check] The kubelet is healthy after 1.006695837s
[control-plane-check] Waiting for healthy control plane components. This can take up to 4m0s
[control-plane-check] Checking kube-apiserver at https://192.168.1.7:6443/livez
[control-plane-check] Checking kube-controller-manager at https://127.0.0.1:10257/healthz
[control-plane-check] Checking kube-scheduler at https://127.0.0.1:10259/livez
[control-plane-check] kube-controller-manager is healthy after 6.020542959s
[control-plane-check] kube-scheduler is healthy after 9.803254865s
[control-plane-check] kube-apiserver is healthy after 2m33.628702362s
[upload-config] Storing the configuration used in ConfigMap "kubeadm-config" in the "kube-system" Namespace
[kubelet] Creating a ConfigMap "kubelet-config" in namespace kube-system with the configuration for the kubelets in the cluster
[upload-certs] Storing the certificates in Secret "kubeadm-certs" in the "kube-system" Namespace
[upload-certs] Using certificate key:
454c19fc6a0b52f8da2398a8063ddccf2913c8b291a878ff0e8e9f5b2b1328b0
[mark-control-plane] Marking the node controlplane as control-plane by adding the labels: [node-role.kubernetes.io/control-plane node.kubernetes.io/exclude-from-external-load-balancers]
[mark-control-plane] Marking the node controlplane as control-plane by adding the taints [node-role.kubernetes.io/control-plane:NoSchedule]
[bootstrap-token] Using token: a5pkyv.efswbfj7b45avi6o
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

kubeadm join 192.168.1.7:6443 --token a5pkyv.efswbfj7b45avi6o \
        --discovery-token-ca-cert-hash sha256:c0a47dfa1e27db218801823269e9f47065f4931d28eb5da7aa12c267f07dd383
vagrant@controlplane:~$

```

- Once done , then exectue mkdir, sudo cp and sudo chown command on controlplane node


**Step 5: Install addons**

- here we are installing calico 

https://kubernetes.io/docs/concepts/cluster-administration/addons/
https://docs.tigera.io/calico/latest/getting-started/kubernetes/self-managed-onprem/onpremises


kubectl create -f https://raw.githubusercontent.com/projectcalico/calico/v3.31.3/manifests/operator-crds.yaml
kubectl create -f https://raw.githubusercontent.com/projectcalico/calico/v3.31.3/manifests/tigera-operator.yaml

curl -O https://raw.githubusercontent.com/projectcalico/calico/v3.31.3/manifests/custom-resources.yaml

- After running the above command, edit the CIDR of above file. CIDR taken from the kubeadm init command

```
10.244.0.0/16

vagrant@controlplane:~$ sudo cat custom-resources.yaml
# This section includes base Calico installation configuration.
# For more information, see: https://docs.tigera.io/calico/latest/reference/installation/api#operator.tigera.io/v1.Installation
apiVersion: operator.tigera.io/v1
kind: Installation
metadata:
  name: default
spec:
  # Configures Calico networking.
  calicoNetwork:
    ipPools:
      - name: default-ipv4-ippool
        blockSize: 26
        cidr: 10.244.0.0/16
        encapsulation: VXLANCrossSubnet
        natOutgoing: Enabled
        nodeSelector: all()
```

- Now apply it

```
kubectl create -f custom-resources.yaml
```

- After a few minutes, all the Calico components display True in the AVAILABLE column.

```
vagrant@controlplane:~$ kubectl get tigerastatus
NAME        AVAILABLE   PROGRESSING   DEGRADED   SINCE
apiserver   True        False         False      88s
calico      True        False         False      108s
goldmane    True        False         False      118s
ippools     True        False         False      5m33s
whisker     True        False         False      63s
vagrant@controlplane:~$

```

**Step 5: Join the nodes**

- Run the command given by kubeadm while initializing, we need to run on node01 and node02, not on controlplane

```
sudo kubeadm join 192.168.1.7:6443 --token a5pkyv.efswbfj7b45avi6o \
        --discovery-token-ca-cert-hash sha256:c0a47dfa1e27db218801823269e9f47065f4931d28eb5da7aa12c267f07dd383
```

**Node01 output**

- it will take about 4 min to get ready
```
vagrant@node01:~$ sudo kubeadm join 192.168.1.7:6443 --token a5pkyv.efswbfj7b45avi6o \
        --discovery-token-ca-cert-hash sha256:c0a47dfa1e27db218801823269e9f47065f4931d28eb5da7aa12c267f07dd383
[preflight] Running pre-flight checks
        [WARNING ContainerRuntimeVersion]: You must update your container runtime to a version that supports the CRI method RuntimeConfig. Falling back to using cgroupDriver from kubelet config will be removed in 1.36. For more information, see https://git.k8s.io/enhancements/keps/sig-node/4033-group-driver-detection-over-cri
[preflight] Reading configuration from the "kubeadm-config" ConfigMap in namespace "kube-system"...
[preflight] Use 'kubeadm init phase upload-config kubeadm --config your-config-file' to re-upload it.
[kubelet-start] Writing kubelet configuration to file "/var/lib/kubelet/instance-config.yaml"
[patches] Applied patch of type "application/strategic-merge-patch+json" to target "kubeletconfiguration"
[kubelet-start] Writing kubelet configuration to file "/var/lib/kubelet/config.yaml"
[kubelet-start] Writing kubelet environment file with flags to file "/var/lib/kubelet/kubeadm-flags.env"
[kubelet-start] Starting the kubelet
[kubelet-check] Waiting for a healthy kubelet at http://127.0.0.1:10248/healthz. This can take up to 4m0s
[kubelet-check] The kubelet is healthy after 517.655713ms
[kubelet-start] Waiting for the kubelet to perform the TLS Bootstrap

This node has joined the cluster:
* Certificate signing request was sent to apiserver and a response was received.
* The Kubelet was informed of the new secure connection details.

Run 'kubectl get nodes' on the control-plane to see this node join the cluster.


vagrant@controlplane:~$ kubectl get nodes
NAME           STATUS     ROLES           AGE     VERSION
controlplane   Ready      control-plane   33m     v1.35.0
node01         Ready      <none>          3m14s   v1.35.0

```

Fix error on worker nodes by following below chatgpt steps

error
```
vagrant@node01:~$ kubectl get pods
E1228 16:16:51.044882    4922 memcache.go:265] "Unhandled Error" err="couldn't get current server API group list: Get \"http://localhost:8080/api?timeout=32s\": dial tcp 127.0.0.1:8080: connect: connection refused"
E1228 16:16:51.046390    4922 memcache.go:265] "Unhandled Error" err="couldn't get current server API group list: Get \"http://localhost:8080/api?timeout=32s\": dial tcp 127.0.0.1:8080: connect: connection refused"
E1228 16:16:51.048887    4922 memcache.go:265] "Unhandled Error" err="couldn't get current server API group list: Get \"http://localhost:8080/api?timeout=32s\": dial tcp 127.0.0.1:8080: connect: connection refused"
E1228 16:16:51.052788    4922 memcache.go:265] "Unhandled Error" err="couldn't get current server API group list: Get \"http://localhost:8080/api?timeout=32s\": dial tcp 127.0.0.1:8080: connect: connection refused"
E1228 16:16:51.052983    4922 memcache.go:265] "Unhandled Error" err="couldn't get current server API group list: Get \"http://localhost:8080/api?timeout=32s\": dial tcp 127.0.0.1:8080: connect: connection refused"

```

**Fix**

This is **expected behavior** and **nothing is broken** 👍
You’re seeing this because of how `kubectl` works on **worker nodes**.

---

## Why this error happens on node01 / node02

```
Get "http://localhost:8080/api": connect: connection refused
```

### Root cause

On **worker nodes**:

* There is **NO kube-apiserver running**
* `kubectl` **does not know** where the API server is
* So it defaults to:

  ```
  http://localhost:8080
  ```
* Which obviously fails ❌

---

## Key concept (very important)

> **`kubectl` is not meant to be used on worker nodes by default**

Worker nodes only run:

* kubelet
* kube-proxy
* container runtime

Control plane runs:

* kube-apiserver
* etcd
* controller-manager
* scheduler

---

## Why it works on the control plane

On the control plane, during `kubeadm init`, you ran:

```bash
mkdir -p $HOME/.kube
sudo cp -i /etc/kubernetes/admin.conf $HOME/.kube/config
sudo chown $(id -u):$(id -g) $HOME/.kube/config
```

That file:

* Tells `kubectl` **where the API server is**
* Contains **certificates and credentials**

Worker nodes **don’t have this file**

---

## ✅ Correct ways to fix / handle this

### ✅ Option 1 (Best practice – recommended)

👉 **Run `kubectl` only on the control plane (or jump host)**

This is how real production clusters work.

---

### ✅ Option 2 (If you really want kubectl on worker nodes)

Copy kubeconfig from control plane:

On **control plane**:

```bash
scp /etc/kubernetes/admin.conf vagrant@node01:/home/vagrant/.kube/config
scp /etc/kubernetes/admin.conf vagrant@node02:/home/vagrant/.kube/config
```

On **node01 / node02**:

```bash
mkdir -p ~/.kube
sudo mv /home/vagrant/.kube/config ~/.kube/config
sudo chown $(id -u):$(id -g) ~/.kube/config
```

Then test:

```bash
kubectl get nodes
```

✅ It will work

⚠️ But this is **not recommended for production** (security risk)

---

### ❌ Option 3 (Wrong)

```bash
kubectl get pods -A --server=localhost
```

❌ API server is NOT on workers

---

## Interview-ready explanation

> *“kubectl fails on worker nodes because kube-apiserver does not run there. Without a kubeconfig file, kubectl defaults to localhost:8080, which results in a connection refused error.”*

---

## Summary

| Question                    | Answer                    |
| --------------------------- | ------------------------- |
| Why error on workers?       | No kube-apiserver         |
| Is cluster broken?          | ❌ No                      |
| Correct place for kubectl   | Control plane / jump host |
| Can kubectl run on workers? | Yes, but not recommended  |

---

You’re doing **everything correctly** 👌
Next good topic to learn from this setup:
👉 *Difference between kubelet vs kubectl*

Say **“explain kubelet vs kubectl”** if you want that breakdown 🚀


