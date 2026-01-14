**`chmod`** is a Linux command used to **change file or directory permissions**.

### In short:

> **`chmod` controls who can read, write, or execute a file.**

---

### Permission types

* **r** → read
* **w** → write
* **x** → execute

### Who gets permissions

* **u** → owner
* **g** → group
* **o** → others

---

### Example

```bash
chmod 644 file.txt
```

Means:

* Owner → read & write
* Group → read
* Others → read

---

### Another example

```bash
chmod 755 script.sh
```

Means:

* Owner → read, write, execute
* Group & Others → read, execute

---

### Common use

* Make script executable
* Restrict file access
* Secure directories

That’s it — simple and essential Linux permission control 👍
