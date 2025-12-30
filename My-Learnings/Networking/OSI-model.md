- It explains the journey of data across the interner ( from client to server ), explaines the entire thing in 7 layers
- L1 to L7 (or) L7 to L1

<img width="1028" height="490" alt="image" src="https://github.com/user-attachments/assets/606c7cfd-90e3-4f08-8a44-56e806e95249" />

- suppose we are searching for google.com in browser, even before it initiates a request two things will happen. those are DNS and TCP handshake
  - DNS resolution
    - Every internet service provider maintains a DNS server where complete records of IP adress and its associated domain will be available
    - So every domain should get mapped with particular ip address. suppose you are searching for www.seshadri.com, then that domain itself not exists
    - When you open google.com in Chrome, where does the router come in?
         ## How router fits in your google.com example
   
      first it will check in local cache, then only it will go to dns sever

    ```
       Laptop → Hathway Router (192.168.1.1) → Internet → Google
      
     ```
           
           
    - TCP handshake
