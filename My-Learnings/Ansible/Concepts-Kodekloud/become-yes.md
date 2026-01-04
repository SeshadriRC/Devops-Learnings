In **Ansible**,

### `become: yes`

means **“run this task/play with elevated (sudo) privileges.”**

---

### In simple terms

It tells Ansible:

> *Log in as the normal user, then switch to root (or another privileged user) to execute commands.*

Equivalent to running:

```bash
sudo <command>
```

---

### Why it’s needed here

In your playbook:

```yaml
become: yes
```

You are:

* Creating `/opt/security`
* Copying a file into `/opt`

These paths are **owned by root**, so a normal user cannot write there.
Without `become: yes`, the playbook would fail with **permission denied**.

---

### How it works internally

By default:

* `become_method`: `sudo`
* `become_user`: `root`

So this:

```yaml
become: yes
```

Is the same as:

```yaml
become: yes
become_method: sudo
become_user: root
```

---

### Where you can use it

You can apply `become` at different levels:

**Play level (recommended):**

```yaml
- hosts: app_servers
  become: yes
  tasks:
```

**Task level:**

```yaml
- name: Copy file
  become: yes
  copy:
    src: file
    dest: /opt/file
```

---

### When you do NOT need it

* Reading files
* Copying to `/tmp`
* Running user-level commands

---

### One-line memory tip 🧠

👉 **If the task needs root permissions → use `become: yes`.**

If you want, I can also explain:

* `become_user`
* `become: no`
* Difference between `become` and `sudo`
  Just tell me 👍
