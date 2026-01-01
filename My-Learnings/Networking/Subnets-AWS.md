### 🧩 What is a Subnet in AWS?

A **subnet** is a **smaller network inside a VPC** where you place your AWS resources (EC2, RDS, Load Balancer).

👉 Think of a subnet as **dividing a big network (VPC) into smaller, manageable networks**.

---

## 🧠 Simple Definition (Interview-ready)

> **A subnet is a range of IP addresses within a VPC that is used to isolate and organize AWS resources.**

---

## 📘 Key Characteristics of a Subnet

### 🔹 Belongs to ONE VPC

* You cannot share a subnet across VPCs

### 🔹 Lives in ONE Availability Zone

* A subnet **cannot span multiple AZs**

### 🔹 Has its own CIDR block

Example:

```
VPC CIDR:    10.0.0.0/16
Subnet CIDR: 10.0.1.0/24
```

---

## 🌐 Public vs Private Subnet

### Public Subnet

* Route table contains:

  ```
  0.0.0.0/0 → Internet Gateway (IGW)
  ```
* Used for:

  * Load Balancers
  * Bastion hosts

### Private Subnet

* No direct route to IGW
* Used for:

  * App servers
  * Databases

⚠️ **Public/Private depends on routing, not IP range**

---

## 🖼️ Simple Diagram

```
VPC (10.0.0.0/16)
│
├── Public Subnet (10.0.1.0/24)  → IGW
│
└── Private Subnet (10.0.2.0/24) → NAT / No Internet
```

---

## 🔐 Security at Subnet Level

* **NACL (Network ACL)** works at subnet level
* **Security Groups** work at resource level

---

## 🎯 Interview One-Liner

> “A subnet is a CIDR block within a VPC that defines where AWS resources are launched and controls their routing and network access.”

---

## 🧠 Easy Memory Tip

* **VPC** = Big network
* **Subnet** = Small network inside VPC
* **Route table** = Traffic rules


