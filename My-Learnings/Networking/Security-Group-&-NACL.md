
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

<img width="1068" height="456" alt="image" src="https://github.com/user-attachments/assets/025d8dd0-8036-4b4c-b718-1e57af04ec1f" />
