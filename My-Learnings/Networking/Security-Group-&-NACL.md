
- In AWS security is a shared responsibility, which means from our end also we need to decide how to configure SG, which type of LB need to be used etc., If we create
  **VPC** in aws by default aws will create IG, NACL, Route table
- SG and NACL are the last point of security in AWS.
- User will have access to loadbalancer which is in **public subnet**
- LB will have access to the application which is in **private subnet**




**Security Group**

- SG is only for allowing
- If we add security to the instance level for eg: EC2 then in this case we will be using security group
- By default AWS won't allow traffic inside the VPC , if we need to access then SG should be configured
- For example we need to access the jenkins application running on port 8080, then we need to configure rules to allow inbound connection to 8080
- There are two things in SG, inbound and outbound traffic.
    - Inbound -> Traffic coming to the EC2 instance ( User accessing Amazon application )
    - Outbound -> Traffic going out of EC2 instance ( Amazon accessing Razorpay application )
 
- Whenever we are creating EC2 instance, aws will assign default SG to the instance . Where by default it will denly all incoming traffic and it will allow all outbound except port 25
- Port 25 is a mailing service so it won't allow outbound, to restrict the spamming activity

**NACL (Network Access Control List)**

- NACL is for Deny and Allow
- we can add security at the subnet level, here we will be using **NACL**
- If some rules set on subnet level, then it will be applied to all the resources inside the subnet.
- Instead of setting security group to resources, we can define **NACL**, so it will get applied to all resources, even if having 10,000+ resources it will use that configuration only


**Practicals**

- In this practical, we created VPC
- Created EC2, edit network configuration for that.
- Install python application on EC2 and try to access


**VPC**
- we created VPC, 2 availability zones

<img width="1068" height="456" alt="image" src="https://github.com/user-attachments/assets/025d8dd0-8036-4b4c-b718-1e57af04ec1f" />

<img width="1879" height="896" alt="image" src="https://github.com/user-attachments/assets/317404c3-450c-4bbe-87f9-32c89691af50" />
<img width="1872" height="577" alt="image" src="https://github.com/user-attachments/assets/45c60971-e41e-4b03-9220-55696533c078" />
<img width="1916" height="833" alt="image" src="https://github.com/user-attachments/assets/a85f336d-a1aa-428e-b74a-c3b11dd2406e" />
<img width="1302" height="888" alt="image" src="https://github.com/user-attachments/assets/9f81e9ca-4845-4e02-b7e0-60492eefa1bf" />
<img width="1862" height="817" alt="image" src="https://github.com/user-attachments/assets/95903dbe-de6d-41f3-927f-eace6ce8d24e" />

- one route table for 2 public subnets
<img width="1521" height="759" alt="image" src="https://github.com/user-attachments/assets/6eb9cf4e-b288-42d0-b183-7d8546796758" />

--------------------------------------------
**EC2**

- Select the custom vpc which we created
- By default, it will use private subnet as per the industry practice, but for this practicals we are selecting public subnet since we are not any LB here
- Auto assin public -> yes
- allow aws to create default SG, then we will tweak it

<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/808a12b4-af0f-4a32-bcaa-d1db2227be05" />
<img width="1919" height="575" alt="image" src="https://github.com/user-attachments/assets/259cdb85-8e06-4b97-8eac-6b93da1c06b6" />

--------------------------------------------
**Install Python App**

- Login to EC2
- Install python

```
ssh -i <private-key> ubuntu@<public-ip>
sudo apt update
python3    -> it installed automatically without doing anything
python3 -m http.server 8000
```

- ideally we should able to access this app, but see here its not getting loaded

<img width="1457" height="214" alt="image" src="https://github.com/user-attachments/assets/aa16216d-a099-4eff-9cd5-6768c7ebab60" />
<img width="1132" height="231" alt="image" src="https://github.com/user-attachments/assets/497fc938-1d4c-4204-b978-4d1e06e74297" />





VPC -> Create VPC
