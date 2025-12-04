`wget` is a **command-line utility** in Linux used to **download files from the internet** using protocols like:

* **HTTP**
* **HTTPS**
* **FTP**

It is very commonly used for downloading software, scripts, packages, and configuration files directly from URLs.

---

# ✅ **What is `wget` in simple terms?**

**`wget` is a tool that downloads files from the web without needing a browser.**

---

# 🔧 **Common Uses of `wget`**

| Purpose                             | Example                                       |
| ----------------------------------- | --------------------------------------------- |
| Download a file                     | `wget https://example.com/file.zip`           |
| Save with a custom name             | `wget -O output.txt https://example.com/data` |
| Download in background              | `wget -b https://example.com/file.iso`        |
| Download entire website (recursive) | `wget -r https://example.com`                 |
| Resume interrupted download         | `wget -c https://example.com/largefile.iso`   |

---

# 📌 **Example (your command)**

You used:

```bash
wget -O /etc/yum.repos.d/jenkins.repo https://pkg.jenkins.io/redhat-stable/jenkins.repo
```

This means:

* **`-O`** → Save the downloaded file with a specific name
* Download **jenkins.repo** from the Jenkins repository URL
* Save it as **/etc/yum.repos.d/jenkins.repo**

This is how we add the Jenkins repository to the system.

---

# 🧠 **Short Interview Answer**

> **`wget` is a Linux command-line tool used for downloading files from the internet using HTTP, HTTPS, or FTP. It is non-interactive, so it can run in scripts, automation, or in the background.**

---

If you want, I can also explain the difference between `curl` and `wget` — a common interview question.
