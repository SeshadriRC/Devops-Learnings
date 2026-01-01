
**Pre-requisites**

- VPC
- Subnets
- Internet Gateway
- Load Balancer
- Route-table



**Land example for VPC**
<img width="1152" height="629" alt="image" src="https://github.com/user-attachments/assets/011eb1ab-cb6c-428b-9365-97212d15f9c0" />


**Devops engineer will create a VPC**

- Inside datacenter there will be multiple servers, On each server we can create multiple vm's
- if hacker hacks one VM in a server, then all the VM's will get affected. so to fix this issue, aws introduced VPC

<img width="927" height="559" alt="image" src="https://github.com/user-attachments/assets/14b8416a-fb96-49b7-b9a1-eb3f448070d0" />


**Flow**

- First we will be creating a **VPC** with CIDR range 172.16.0.0/16, so totally 65536 ip addresses will be allocated
- So inside VPC we will divide the ip adrress for sub projects, that is call **subnets**
- 

<img width="1181" height="596" alt="image" src="https://github.com/user-attachments/assets/12f6c18a-7ded-4f63-90e1-462d0844f6e4" />

