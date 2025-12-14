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

**Step 3**
- for static code analysis install sonarqube

<img width="1507" height="837" alt="image" src="https://github.com/user-attachments/assets/a2e93163-a16b-44a5-9eda-f55fe03bf81d" />

<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/5cbe4ff4-d583-456c-9b4b-1399a6fd0e34" />

- click manually

<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/776f2350-5e74-4804-8e65-1c76cc1864ef" />
<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/53330583-0b92-4e80-b048-cf3936653e1f" />

- select with jenkins

<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/63f86842-9862-46ac-a93f-7ad7a82d7e88" />

- then select github

<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/a84b16b8-1f91-4d4d-952e-93c09b5896db" />

- configure analysis, then click continue for git webhook also, then select maven and finish tutorial

<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/5594f299-46eb-4c11-be77-cf9abec24bc3" />
<img width="1310" height="410" alt="image" src="https://github.com/user-attachments/assets/8abc53e7-3f59-4aaa-8de4-7a83bcd781a7" />

<img width="1493" height="543" alt="image" src="https://github.com/user-attachments/assets/3b5281f3-4294-46e6-86c5-b6192801fb7a" />
<img width="857" height="365" alt="image" src="https://github.com/user-attachments/assets/ea159990-c1d7-4c20-90bb-4ae040d02bd0" />
<img width="1563" height="618" alt="image" src="https://github.com/user-attachments/assets/b66182d5-0e6f-43c8-b0f4-35b0ebf9f378" />


- go to myaccount -> security -> enter token name and generate it

<img width="1390" height="626" alt="image" src="https://github.com/user-attachments/assets/cbe13fea-e919-456c-a6da-a821f3ec4207" />
<img width="1240" height="484" alt="image" src="https://github.com/user-attachments/assets/ad664aa2-66aa-4854-8010-eecda53c2b99" />

