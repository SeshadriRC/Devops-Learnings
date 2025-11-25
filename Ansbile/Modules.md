## Template  

**100 days of devops - Day 92**

The **template module** in Ansible is used to **copy files to remote servers after processing Jinja2 variables inside them**.

### ⭐ **Simple Definition**

The **template module** reads a `.j2` (Jinja2) file from your control machine, replaces variables (like `{{ inventory_hostname }}`, `{{ ansible_user }}`, etc.), and then creates a final file on the target server.

---

# 🔍 **Why do we need the template module?**

Use the template module when:

* You want dynamic content
* File content should change based on host
* You want to avoid hardcoding values
* You want a template that is reusable for many servers

Example: index.html should show the server name automatically.

---

# 🔧 **Example**

Template file: `index.html.j2`

```
This is server {{ inventory_hostname }}
```

Task:

```yaml
- name: deploy template
  template:
    src: index.html.j2
    dest: /var/www/html/index.html
```

If the playbook runs on:

* stapp01 → creates file: “This is server stapp01”
* stapp02 → creates file: “This is server stapp02”
* stapp03 → creates file: “This is server stapp03”

**That’s the power of templates.**

---

# 🔍 **Where templates live in roles**

```
roles/
  myrole/
    templates/
      index.html.j2
```

Ansible automatically looks inside the **templates/** directory.

---

# 📌 **Important: template module vs copy module**

| Feature                     | template module | copy module |
| --------------------------- | --------------- | ----------- |
| Supports variables (Jinja2) | ✔ Yes           | ❌ No        |
| Dynamic content             | ✔               | ❌           |
| File extension              | Usually `.j2`   | Any         |
| Static files                | Okay            | Best        |
| Performance                 | Good            | Best        |

---

# ⭐ Final Summary

The **template module**:

* Reads `.j2` file (Jinja2 template)
* Replaces variables
* Creates the final file on the target machine
* Ensures correct owner, group, permissions

It is used for **dynamic, host-specific, configurable files**.

---

If you want, I can show a step-by-step example with variables or explain Jinja2 in detail.
