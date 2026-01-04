
Refer this [practicals](https://github.com/SeshadriRC/Devops-Learnings/blob/main/My-Learnings/Networking/AWS-VPC-handson.md) to understand clearly

**Pre-requisites**

- [VPC](https://github.com/SeshadriRC/Devops-Learnings/blob/main/My-Learnings/Networking/VPC.md)
- [Subnets-AWS](https://github.com/SeshadriRC/Devops-Learnings/blob/main/My-Learnings/Networking/Subnets-AWS.md)
- [Internet Gateway](https://github.com/SeshadriRC/Devops-Learnings/blob/main/My-Learnings/Networking/Internet-gateway.md)
- [Load Balancer](https://github.com/SeshadriRC/Devops-Learnings/tree/main/My-Learnings/Networking)
- [Security-group]
- Route-table
- NAT gateway



**Land example for VPC**
<img width="1152" height="629" alt="image" src="https://github.com/user-attachments/assets/011eb1ab-cb6c-428b-9365-97212d15f9c0" />


**Devops engineer will create a VPC**

- Inside datacenter there will be multiple servers, On each server we can create multiple vm's
- if hacker hacks one VM in a server, then all the VM's will get affected. so to fix this issue, aws introduced VPC

<img width="927" height="559" alt="image" src="https://github.com/user-attachments/assets/14b8416a-fb96-49b7-b9a1-eb3f448070d0" />


**Flow**

- First we will be creating a **VPC** with CIDR range 172.16.0.0/16, so totally 65536 ip addresses will be allocated
- So inside VPC we will divide the ip adrress for sub projects, that is call **subnets**
- First user will connect to public subnet using internet gateway

<img width="1181" height="596" alt="image" src="https://github.com/user-attachments/assets/12f6c18a-7ded-4f63-90e1-462d0844f6e4" />



- user is trying to access 172.16.31(app) which resides in the private subnet, usually that ip should be LB ip, so for our understanding
  Abi is explaining with private ip
  
- Setup
   - VPC is there , IG is attached to it.
   - VPC having common CIDR IP[**172.16.0.0/16**], inside that we are creating 3 subnets for project A,B,c and for each project we have separated IP address range [**172.16.1.0/24,172.16.2.0/24, 172.16.3.0/24**] from VPC common CIDR
   
- Our goal is to reach ip **172.16.1.0/24** which resides in the private subnet
- First connection will pass through the **IG** --> **Public subnet**( this can be accessed by public outside the VPC), but first they need to pass through IG
- Then LB will be inside the public subnet, from the **ELB** connection will route to the application we have requested
- How ELB knows to which application we need to reach, it will reach by using **Route**, 
  - who will define this route ? there is something called as route table ( nothing but a router )
  - so to the ELB we will attach the private subnet and **target group**
  - so first we need to create target group and need to assign EC2 instance which present in the **Private Subnet** to the target group and private subnet
    shud have the **route table** so that traffic can flow here
- Then there will be a **security group** attached to ec2 instance which can block or allow the request
      - if incase within a subnet if we need to configure same SG settings for all instances, then we can use NACL, it is a automation for security group
- Suppose **172.16.1.0** need to access the internet for downloading packages or some other thing, then here we will use **NAT gateway** so that IP won't get exposed to the public. It is a bad practice to expose the private ip to the public
