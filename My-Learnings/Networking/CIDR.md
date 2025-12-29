- **CIDR (Classless Inter-Domain Routing)** is a method of **IP addressing and routing** that uses a prefix length (like `/24`) to define **flexible IP ranges instead of fixed classes**.

- Whenever we are creating VPC in AWS we will be providing CIDR range there. Eg: 192.16.0.0/16, it will provide 65,536 ip addresses as per online calculator cidr

- Now out of that 65,536 ip address, user needs only 32 ip addresses. so below will be the calculation
  
  Total bits is **32** for ipv4 , so we already know 2^5 is 32. so 32-5=27

  **Ans:** 192.16.0.0/27

- Now calculate for 172.168.3.0/30 and 10.0.0.0/8

- For 172.168.3.0/30 

    32-30=2, so 2^2 = 4  -> so we can use **4** ip address here

- For 10.0.0.0/8

   32-8=24, so 2^24 = 16,777,216 -> so we can use **16,777,216** ip address here
