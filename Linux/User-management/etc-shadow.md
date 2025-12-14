`/etc/shadow` is a **critical Linux security file** that stores **encrypted user password information** and password-aging policies.

---

## 🔐 Why `/etc/shadow` exists

Earlier Linux systems stored passwords in `/etc/passwd`, which is **world-readable**.
To improve security, passwords were moved to `/etc/shadow`, which is **readable only by root**.

---

## 📁 File details

```bash
ls -l /etc/shadow
```

Typical permissions:

```
-r-------- 1 root root /etc/shadow
```

👉 Only **root** (or processes with CAP_DAC_OVERRIDE) can read it.

---

## 🧱 `/etc/shadow` format

Each line has **9 colon-separated fields**:

```
username:password:lastchg:min:max:warn:inactive:expire:reserved
```

### Example

```
jarod:$6$Qz9...hash...:19680:0:99999:7:::
```

---

## 🔍 Field explanation

| # | Field    | Meaning                                         |
| - | -------- | ----------------------------------------------- |
| 1 | username | Login name                                      |
| 2 | password | **Hashed password** or special value            |
| 3 | lastchg  | Days since 1970-01-01 password was last changed |
| 4 | min      | Min days before password can be changed         |
| 5 | max      | Max password age                                |
| 6 | warn     | Warning days before expiry                      |
| 7 | inactive | Days after expiry before account disabled       |
| 8 | expire   | Account expiration date                         |
| 9 | reserved | Unused                                          |

---

## 🔑 Special values in password field

| Value    | Meaning                 |
| -------- | ----------------------- |
| `$6$...` | SHA-512 hashed password |
| `!`      | Account locked          |
| `!!`     | Password never set      |
| `*`      | Login disabled          |

---

## 🧪 Useful commands (safe)

### View password status (without showing hash)

```bash
sudo passwd -S jarod
```

### Check aging info

```bash
sudo chage -l jarod
```

### Lock / unlock user

```bash
sudo passwd -l jarod
sudo passwd -u jarod
```

---

## 🔄 Relationship with `/etc/passwd`

| File          | Contains                       |
| ------------- | ------------------------------ |
| `/etc/passwd` | Username, UID, GID, shell      |
| `/etc/shadow` | Password hash, expiry policies |

`/etc/passwd` shows:

```
jarod:x:1002:1003::/home/jarod:/bin/bash
```

`x` → password is stored in `/etc/shadow`

---

## 🧠 One-line summary

👉 **`/etc/shadow` securely stores hashed passwords and account expiry rules, readable only by root.**

If you want, I can also explain:

* Difference between `!` and `*`
* How password expiry works
* How this ties into LDAP / SSSD
