
# Q.1 what is nat gateway and why it is used in public subnet. give me short answer

**NAT Gateway — short and clear** 👇

> **A NAT Gateway allows resources in a private subnet to access the internet, without allowing inbound internet access to them.**

### Why it is in a public subnet

* NAT Gateway **needs internet access**
* Public subnet has route:

  ```
  0.0.0.0/0 → Internet Gateway
  ```
* Private subnet routes outbound traffic to:

  ```
  0.0.0.0/0 → NAT Gateway
  ```

### One-line interview answer

> “A NAT Gateway is placed in a public subnet so private subnet resources can access the internet securely without being exposed to inbound traffic.”

---

# Q.2 So which means it will mask the ip address of private subnet right ?

Yes ✅ **exactly right.**

👉 **NAT Gateway masks the private subnet IP addresses.**

### How it works (short)

* EC2 in **private subnet** sends traffic
* Traffic goes to **NAT Gateway**
* NAT Gateway **replaces private IP** with its **public IP**
* Internet sees **only the NAT Gateway’s public IP**

### Key points

* **Outbound only** for private subnets
* **No inbound internet traffic** allowed
* Private IPs are **hidden from the internet**

### One-line interview answer

> “A NAT Gateway masks private subnet IPs by translating them to its public IP for outbound internet access.”

Perfect understanding 👍

---
