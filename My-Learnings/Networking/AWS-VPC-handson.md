<img width="1778" height="766" alt="image" src="https://github.com/user-attachments/assets/2dd86336-4adb-4c63-8b2e-fa05f060133c" />

<img width="1575" height="912" alt="image" src="https://github.com/user-attachments/assets/9e851472-22bc-4023-982a-e7e90919509d" />

<img width="1691" height="652" alt="image" src="https://github.com/user-attachments/assets/ccbdcac7-978e-490f-875a-a2c7b2620b5e" />



<img width="1269" height="656" alt="image" src="https://github.com/user-attachments/assets/8fcf01aa-d30f-4637-8d13-b7b99d2d3a44" />



- Create VPC and more
- Create Autoscaling group
- Create Bastion host and access other servers
- Install python application on one ec2 instance
- Create load balancer and add two ec2 instance as target group


- VPC creation
<img width="461" height="576" alt="image" src="https://github.com/user-attachments/assets/e0d4eb31-2ec2-405e-863a-a0055ace070d" />
<img width="1093" height="427" alt="image" src="https://github.com/user-attachments/assets/6cf6ac0d-1c4b-4ff3-902e-4cd20d138548" />
<img width="1113" height="332" alt="image" src="https://github.com/user-attachments/assets/c800e475-3963-4b04-91c9-ef3ec42d9bc2" />
<img width="1093" height="472" alt="image" src="https://github.com/user-attachments/assets/a32eb29c-ad95-43ac-8eff-4772f7b7c399" />
<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/9a3620b7-1e70-43e2-b976-e89cbcbec4e5" />
<img width="1844" height="518" alt="image" src="https://github.com/user-attachments/assets/fecd8381-787f-42db-bc96-fb750277900e" />

What is [Elastic ip](https://github.com/SeshadriRC/Devops-Learnings/blob/main/My-Learnings/AWS/Elastic-ip.md)? here elastic ip is used by NAT gateway

  - Public subnet is attached to the route table and internet gateway
    <img width="1441" height="594" alt="image" src="https://github.com/user-attachments/assets/098762f8-7cad-44ae-b751-3583346ef704" />

    
  - Private subnet is attached to different route tables
    <img width="1384" height="527" alt="image" src="https://github.com/user-attachments/assets/150f6d33-fb8b-4b85-bc6c-ad47f59182bc" />

- Autoscaling group creation
    - click on create autoscaling group
  <img width="1870" height="500" alt="image" src="https://github.com/user-attachments/assets/2cb07ff2-5a3f-4946-b3ea-5121f8ff0136" />

    - we can't create the ASG directly in AWS, first we need launch template for that, so please create that first
  <img width="1858" height="689" alt="image" src="https://github.com/user-attachments/assets/9ae2ced5-48be-4424-b40b-d0b2071bcbe0" />
  <img width="889" height="295" alt="image" src="https://github.com/user-attachments/assets/9730501a-909e-4645-a503-37e08c06e105" />

    - select AMI
  <img width="1036" height="547" alt="image" src="https://github.com/user-attachments/assets/dbaea031-1ef1-416a-8616-c91118cf5b4a" />
  <img width="1799" height="711" alt="image" src="https://github.com/user-attachments/assets/21fd2494-4c78-448b-80b0-5cce18fd0a75" />

  - select instance type
  <img width="952" height="299" alt="image" src="https://github.com/user-attachments/assets/f8ec2bc1-9f4b-4233-8340-ed6c9915e301" />

  - key value pair to use
  <img width="1016" height="261" alt="image" src="https://github.com/user-attachments/assets/c1c69d6d-14aa-4b55-9e0b-61cb2ac1b7b6" />

  - create SG and select the VPC which we have created. so 2 SG's we are creating one for ssh and another is one for the app which deployed on EC2
  <img width="1053" height="723" alt="image" src="https://github.com/user-attachments/assets/bdc8e750-b5dd-4e76-9f63-e0758c2f4ddf" />
  <img width="1055" height="441" alt="image" src="https://github.com/user-attachments/assets/3fa0200b-6373-4135-b8ab-5ac15e147ea8" />
  <img width="943" height="329" alt="image" src="https://github.com/user-attachments/assets/78f260d2-287f-4c12-8657-fa9bfa644170" />
  <img width="1016" height="405" alt="image" src="https://github.com/user-attachments/assets/adae3a68-7e2e-47ed-93a7-8c335405d95e" />

  - No need to add any EBS volumes, just click launch template
  <img width="1559" height="813" alt="image" src="https://github.com/user-attachments/assets/35ac1254-3ff2-4610-8377-d72ddd7c40c0" />

  - Now select the launch template in ASG group
  <img width="1710" height="765" alt="image" src="https://github.com/user-attachments/assets/9877b6ce-ae49-4d76-ae6f-903ef39b8bde" />
  <img width="1483" height="832" alt="image" src="https://github.com/user-attachments/assets/3f7db0a2-c1c2-4032-8378-b0e134d4fcae" />


- go to step 2 , select VPC and private subnets -> here only ec2 instance need to get created

  <img width="1846" height="681" alt="image" src="https://github.com/user-attachments/assets/c0a546ae-9daf-4aa5-867e-9f37d07487f9" />
  <img width="1800" height="810" alt="image" src="https://github.com/user-attachments/assets/d33689b2-046e-476f-963e-82f86626bf29" />

- Now click next

  <img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/4ff34e6d-2488-4999-90a7-357e091541a5" />

- skip LB, healtchecks then click next

 <img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/b723a65e-1553-45b9-a94f-b9bb45cdbb28" />

- select capacity, scaling policy is none

<img width="1812" height="837" alt="image" src="https://github.com/user-attachments/assets/32923f30-e831-4245-8396-a19676f4ef31" />

- skip notifications

<img width="1822" height="558" alt="image" src="https://github.com/user-attachments/assets/35cd5fc2-5be7-4f3a-b21d-97add28607a0" />

- review it and create ASG

<img width="1744" height="849" alt="image" src="https://github.com/user-attachments/assets/e5f02131-7cca-4311-83b0-1814d858c483" />

- ASG created

<img width="1855" height="403" alt="image" src="https://github.com/user-attachments/assets/3c28504b-1539-459b-9899-c5297d779f62" />

- EC2 instances got created

- one instance running in az-a and other is az-b

<img width="1835" height="748" alt="image" src="https://github.com/user-attachments/assets/add6619e-55ad-4584-bfbe-9c67110743cf" />

- we can see instance dont have public ip address, because we have created this instance in private subnets so to access this we need to create bastion host (it acts as a mediator between private subnet and user or public subnet)

<img width="1801" height="575" alt="image" src="https://github.com/user-attachments/assets/979bf629-813f-4411-b17b-5abbb9b25cd6" /> 


---

**Create bastion host**

<img width="1855" height="615" alt="image" src="https://github.com/user-attachments/assets/3e4e08b0-cc98-48cc-acbb-f934b41cfe9c" />
<img width="1084" height="284" alt="image" src="https://github.com/user-attachments/assets/e01d2c6a-f5ca-4d09-acdd-7df972ce9200" />
<img width="1126" height="763" alt="image" src="https://github.com/user-attachments/assets/d27dc0c4-1dfd-4534-b1d0-666057db3fd7" />
<img width="1794" height="400" alt="image" src="https://github.com/user-attachments/assets/6185813d-b15f-4801-9f14-851e0cebd711" />
<img width="1033" height="320" alt="image" src="https://github.com/user-attachments/assets/ee82b912-a19e-4bcf-ab6b-071cf1f92c41" />

- network settings

<img width="1157" height="642" alt="image" src="https://github.com/user-attachments/assets/8bf3ba00-d35d-485e-88ce-6f57b743f1e6" />
<img width="951" height="131" alt="image" src="https://github.com/user-attachments/assets/f1270853-044d-4827-ade1-2b77c985fb8d" />
<img width="1054" height="647" alt="image" src="https://github.com/user-attachments/assets/71635327-3a23-4950-a4d3-38ab1d6e120b" />

- launch the instance

<img width="1839" height="542" alt="image" src="https://github.com/user-attachments/assets/bfa5d1e9-e1f5-4ab2-97ab-6e5b4a96e895" />

- bastion host is created

<img width="1855" height="423" alt="image" src="https://github.com/user-attachments/assets/5391a93c-5aab-4ed2-8004-3f83e22a941f" />

- copy pem file from local computer to bastion host

<img width="1689" height="352" alt="image" src="https://github.com/user-attachments/assets/0efbe711-bb30-4dc9-af23-eafbc22ed0ca" />

- Now you logged into bastion host

<img width="1736" height="885" alt="image" src="https://github.com/user-attachments/assets/30f0a875-ad56-475e-8cc3-ea88498b327a" />

- Now take the private ip of one of ec2 instance and try to login from bastion host

<img width="1699" height="178" alt="image" src="https://github.com/user-attachments/assets/6a9494b0-52c7-4c7c-9e65-0f7373fa3d7b" />

---

**Install Python application**

- Take html page from w3 schools

<img width="1399" height="296" alt="image" src="https://github.com/user-attachments/assets/8929f640-9db2-4be0-8eb2-c43699df2134" />

- Run the application

<img width="1447" height="194" alt="image" src="https://github.com/user-attachments/assets/72be6f48-6daa-4371-af77-c130e95e75e1" />


---

**Create App LB**

<img width="1847" height="612" alt="image" src="https://github.com/user-attachments/assets/a75a8c27-f62d-4d9f-948f-43ae491e6f70" />



s





