
**Router**

The Wi-Fi box provided by your ISP (Hathway in your case)

Usually has:

Antennas

Lights labeled Power, Internet, LAN, Wi-Fi

Often combines router + modem + Wi-Fi AP

**Switch**

If you have multiple wired devices, you might have a separate small box with multiple Ethernet ports

In many home setups, the switch is built into the router (LAN ports act as a Layer 2 switch)

+++++++++++++++++++++++++++++++++++++++++++++++++++
Perfect — from your `ipconfig` output, we can **identify the router, switches, and MAC addresses**. Let me explain clearly.

---

## 1️⃣ **Router (Default Gateway)**

* Look for **Default Gateway** in the active interface.
* Your active Wi-Fi interface:

```
Wireless LAN adapter Wi-Fi:
    IPv4 Address. . . . . . . . . . . : 192.168.1.5
    Default Gateway . . . . . . . . . : 192.168.1.1
```

* **192.168.1.1** → Your **home router (Hathway Wi-Fi box)**
* This router connects your LAN to the internet and forwards packets.

---

## 2️⃣ **Switches**

* In most **home networks**, switches are **built into the router**.
* If you have multiple Ethernet ports on your router, those ports act as **LAN switches**.
* There may not be a visible standalone switch unless you added one.

**Key point:**

> Switches are usually **transparent**, you don’t see them in `ipconfig`. They just forward Ethernet frames based on MAC addresses.

---

## 3️⃣ **MAC Address (Your device’s network interface)**

* To see the MAC address in Windows:

```cmd
getmac
```

or in `ipconfig /all` (shows MAC as **Physical Address**):

```
Wireless LAN adapter Wi-Fi:
   Physical Address. . . . . . . . . : 1C-1B-0D-5A-2F-3C
```

* This is **your laptop’s NIC unique identifier**.
* Used by switches to forward frames on your LAN.

---

### ✅ Quick summary from your setup

| Component     | Where to see                          | Example              |
| ------------- | ------------------------------------- | -------------------- |
| Router (home) | Default Gateway                       | 192.168.1.1          |
| Switch        | Inside router LAN ports (transparent) | Not visible directly |
| MAC Address   | `getmac` or `ipconfig /all`           | 1C-1B-0D-5A-2F-3C    |

---
