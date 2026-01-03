An **Elastic IP address (EIP)** in AWS is a **static, public IPv4 address** designed for cloud resources—mainly **EC2 instances**.

### Simple definition

👉 **Elastic IP = fixed public IP in AWS that you own until you release it**

---

### Why Elastic IP is used

* Public IPs of EC2 instances **change** when you stop/start the instance
* Elastic IP **does not change**
* You can **remap** it to another instance anytime

---

### Key points

* Works only in a **VPC**
* Associated mainly with **EC2**, **NAT Gateway**, or **Network Interface**
* Stays the same even if the instance restarts
* Can be moved quickly during **failover**

---

### Example use case

* You have a **web server**
* DNS points to an **Elastic IP**
* If the server fails, attach the same EIP to a new EC2 instance
  👉 Users continue accessing the service without DNS changes

---

### Cost note ⚠️

* **Free** when attached to a running EC2 instance
* **Charged** if:

  * Not attached to any resource
  * Attached to a stopped instance
  * You have more than one EIP per instance

---

### Elastic IP vs Public IP

| Feature            | Elastic IP | Public IP |
| ------------------ | ---------- | --------- |
| Static             | ✅ Yes      | ❌ No      |
| Changes on restart | ❌ No       | ✅ Yes     |
| Can be reassigned  | ✅ Yes      | ❌ No      |
| Cost               | Sometimes  | Free      |

---

### One-line summary (interview ready)

> **Elastic IP is a static public IPv4 address in AWS that can be attached, detached, and reassigned to EC2 instances for high availability and failover.**

If you want, I can also explain **how EIP works with NAT Gateway** or **steps to allocate & attach one**.
