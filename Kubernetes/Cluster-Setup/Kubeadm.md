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

**Step 3: Initialize the controlplane node**

