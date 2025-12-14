In Linux, **primary** and **secondary** groups define how a user belongs to groups and how file permissions are applied.

---

## 🔹 Primary Group

**Definition:**
The **default group** assigned to a user at login.

### Key points

* Stored in `/etc/passwd` (the **GID** field)
* A user can have **only ONE primary group**
* New files created by the user belong to this group by default

### Example

```bash
jarod:x:1002:1003::/home/jarod:/bin/bash
```

Here:

* **Primary GID = 1003**
* Group name:

```bash
getent group 1003
```

### File creation example

```bash
touch test.txt
ls -l test.txt
```

Output:

```
-rw-r--r-- 1 jarod nautilus_developers test.txt
```

---

## 🔹 Secondary Group (Supplementary Group)

**Definition:**
Additional groups that give the user extra permissions.

### Key points

* Stored in `/etc/group`
* A user can belong to **multiple secondary groups**
* Used mainly for **access control**
* Does NOT affect default group ownership of new files

### Example

```bash
id jarod
```

Output:

```
uid=1002(jarod) gid=1003(nautilus_developers) groups=1003(nautilus_developers),27(sudo)
```

Here:

* `sudo` is a **secondary group**

---

## 🔹 Comparison Table

| Feature            | Primary Group     | Secondary Group |
| ------------------ | ----------------- | --------------- |
| Number allowed     | One only          | Multiple        |
| Stored in          | `/etc/passwd`     | `/etc/group`    |
| Default file group | ✅ Yes             | ❌ No            |
| Permission usage   | Default ownership | Extra access    |
| Login requirement  | Mandatory         | Optional        |

---

## 🔹 Why secondary groups matter

Example:

* App files owned by group `appteam`
* User added as secondary group
* User gets access **without changing ownership**

```bash
usermod -aG appteam jarod
```

---

## 🔹 Can you change primary group?

Yes:

```bash
usermod -g nautilus_developers jarod
```

⚠️ Be careful — this affects file ownership behavior.

---

## 🔹 Quick check commands

```bash
id jarod
groups jarod
getent passwd jarod
```

---

## 🧠 One-line summary

👉 **Primary group = default ownership group**
👉 **Secondary groups = extra access rights**

If you want, I can also explain this with:

* Real-world permission scenario
* Kubernetes/OpenShift analogy
* How `newgrp` works
