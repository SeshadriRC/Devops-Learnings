- It explains the journey of data across the interner ( from client to server ), explaines the entire thing in 7 layers
- L1 to L7 (or) L7 to L1

<img width="1028" height="490" alt="image" src="https://github.com/user-attachments/assets/606c7cfd-90e3-4f08-8a44-56e806e95249" />

- suppose we are searching for google.com in browser, even before it initiates a request two things will happen. those are **DNS and TCP** handshake
  <img width="1132" height="512" alt="image" src="https://github.com/user-attachments/assets/7f786e79-222d-4ff8-8042-ae9ec805d0c7" />

- if both succeeds only then data request initiation starts
  
  - DNS resolution
    
    - Every internet service provider maintains a DNS server where complete records of IP adress and its associated domain will be available
    - So every domain should get mapped with particular ip address. suppose you are searching for www.seshadri.com, then that domain itself not exists
    - When you open google.com in Chrome, where does the [router](https://github.com/SeshadriRC/Devops-Learnings/blob/main/My-Learnings/Networking/router.md) come in?
         ## How router fits in your google.com example
   
    - When I access google.com, the browser first checks its local DNS cache. If not found, the DNS request is sent to a DNS server via the router, which only forwards the traffic. After resolution, the browser connects to Google using the returned IP address.

    ```
       Laptop → Hathway Router (192.168.1.1) → Internet → Google
      
    ```
             
  - TCP handshake
      
    **TCP three-way handshake** is the process used to **establish a reliable connection** between a client and server. there are 2-way and 4-way handshake as well, but we are concentrating only on 3-way because that is a most used one
        
    **Three steps:**
        
       1. **SYN** – Client requests a connection
       2. **SYN-ACK** – Server acknowledges and agrees
       3. **ACK** – Client confirms
        
      **One-line interview answer:**
        
    > *TCP three-way handshake establishes a reliable connection using SYN, SYN-ACK, and ACK before data transfer begins.*


**where the router comes here**

In the **TCP three-way handshake, the router does not participate in the handshake logic** — it **only forwards the packets**.

### Where the router fits (simple view)

```
Client ──SYN──> Router ──SYN──> Server
Client <─SYN-ACK─ Router <─SYN-ACK─ Server
Client ──ACK──> Router ──ACK──> Server
```

### Key points (interview-ready)

* **Handshake happens between client and server**
* **Router is just a transit device**
* Router **does not create, modify, or acknowledge TCP packets**
* Router forwards packets based on **IP routing**

**One-liner:**

> *During the TCP handshake, the router only forwards SYN, SYN-ACK, and ACK packets and does not take part in the handshake itself.*


---
