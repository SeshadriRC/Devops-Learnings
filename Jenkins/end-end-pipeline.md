Main Repo:- https://github.com/gashok13193/end-to-end
Repo2:- https://github.com/gashok13193/DevOps-Docs

<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/570840c5-42dc-4c25-b4ee-cabb37b5c396" /><img width="1690" height="706" alt="image" src="https://github.com/user-attachments/assets/10863588-4b4f-427a-bb8f-dc4a0315d5ec" />



- we need to install jenkins
- allow inboud in security group

**Step1:** git checkout
git creds give in pipeline syntax --> git, then it will generate a code
plugins: stage view

<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/859dcfa6-6ff5-49e8-ba72-a8a04f6ec3a8" />
<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/e38d7a1a-25c4-40c7-8094-eacc8dae5637" />


**step2: Code Compile**

**Pre-requisites**

install maven on ec2 server

<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/2866a469-3edb-4dda-98d3-3691c979fdb5" />


plugin install on jenkins: eclipse temurin installer, maven integration

- Manage Jenkins --> Tools --> Add Maven , then configure Maven

<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/c623e73e-f028-4d45-bfdf-bb0e2f6eb569" />


- Manage Jenkins --> Tools --> Add JDK , then configure JDK - select JDK 17 as we installed JDK 17. then save it

<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/99f3192b-900f-4155-a95c-32ce93cb89fe" />

- Now configure the job and add maven, jdk env varaible in tools block in pipeline script

<img width="1617" height="582" alt="image" src="https://github.com/user-attachments/assets/af0f66b3-d6be-422c-a7ca-0d43b281baad" />

**Compile with maven step**

Add below and save it

<img width="1202" height="555" alt="image" src="https://github.com/user-attachments/assets/6366a6c5-c450-4930-8c5b-f85c0a4f6fda" />
