`nslookup` is a **DNS troubleshooting and query tool**.
It is used to **check how a hostname or domain name is resolved by a DNS server**.

---

## 🔹 What `nslookup` is used for

### 1️⃣ Resolve hostname → IP address

```bash
nslookup stapp03
```

Tells you:

* Which IP DNS returns for the hostname
* Which DNS server answered the query

---

### 2️⃣ Resolve IP address → hostname (reverse lookup)

```bash
nslookup 172.16.238.12
```

---

### 3️⃣ Check which DNS server is being used

Output shows:

```
Server: 127.0.0.11
Address: 127.0.0.11#53
```

Useful to know if:

* System DNS
* Docker DNS
* Kubernetes / OpenShift DNS
  is being used

---

### 4️⃣ Troubleshoot DNS issues

Use `nslookup` when:

* Application cannot resolve hostnames
* Inter-server communication fails
* Verifying DNS entries after changes

---

### 5️⃣ Query specific DNS records

```bash
nslookup -type=A example.com      # IPv4
nslookup -type=AAAA example.com   # IPv6
nslookup -type=MX example.com     # Mail servers
```

---

### 6️⃣ Query a specific DNS server

```bash
nslookup stapp03 8.8.8.8
```

Checks resolution from a **particular DNS server**

---

## 🔹 What `nslookup` does NOT do

❌ It does **not** use `/etc/hosts`
❌ It does **not** test connectivity
❌ It does **not** tell which IP you should SSH into

---

## 🔹 nslookup vs other tools

| Tool         | Uses /etc/hosts | Uses DNS | Best for                  |
| ------------ | --------------- | -------- | ------------------------- |
| nslookup     | ❌               | ✅        | DNS troubleshooting       |
| dig          | ❌               | ✅        | Advanced DNS queries      |
| getent hosts | ✅               | ✅        | System-level resolution   |
| ping         | ✅               | ✅        | Connectivity + resolution |

---

## 🧠 One-line summary

👉 **`nslookup` is used to query DNS servers and verify name-to-IP (or IP-to-name) resolution — nothing more.**

If you want, I can also explain:

* `nslookup` vs `dig`
* Why `nslookup` shows container IPs in your setup
* When to use `getent hosts` instead
