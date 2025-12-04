`yum` stands for **Yellowdog Updater, Modified**.
It is a **package manager** used on **RHEL-based Linux distributions** like:

* Red Hat Enterprise Linux (RHEL)
* CentOS
* Fedora (older versions)
* Oracle Linux
* Amazon Linux 1

---

# ✅ **What is YUM in Linux?**

**YUM is a command-line tool used to install, update, remove, and manage software packages** on RHEL-based systems.
It automatically handles:

* Dependency resolution
* Downloading packages from configured repositories
* Installing required libraries

---

# 🔧 **What YUM Does**

| Task                        | Description                  |
| --------------------------- | ---------------------------- |
| **Install packages**        | `yum install <package>`      |
| **Update packages**         | `yum update` / `yum upgrade` |
| **Remove packages**         | `yum remove <package>`       |
| **Search packages**         | `yum search <package>`       |
| **List installed packages** | `yum list installed`         |
| **Clean cache**             | `yum clean all`              |

---

# 📌 **Example**

To install Jenkins repo and upgrade packages:

```bash
yum install git
yum update -y
yum install jenkins
```

YUM will:

* Resolve all dependencies
* Check configured repos
* Install required RPM packages

---

# 🧠 **Short Interview Answer**

> **YUM (Yellowdog Updater Modified) is a package manager used in RHEL-based Linux systems to install, update, remove, and manage software packages. It automatically handles dependency resolution and retrieves packages from configured repositories.**

---

If you want, I can also explain **DNF**, the newer version of YUM.
