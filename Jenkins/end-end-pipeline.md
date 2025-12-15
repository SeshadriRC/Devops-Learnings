Main Repo:- https://github.com/gashok13193/end-to-end

Repo2:- https://github.com/gashok13193/DevOps-Docs

<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/570840c5-42dc-4c25-b4ee-cabb37b5c396" /><img width="1690" height="706" alt="image" src="https://github.com/user-attachments/assets/10863588-4b4f-427a-bb8f-dc4a0315d5ec" />



- we need to install jenkins
- allow inboud in security group

**Step1:** git checkout
select as pipeline type job
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

now copy this green colour token and keep it with you

now install plugin: Sonarqube scanner

go to tools --> Add sonarqube scanner and give any name, but before that since sonarqube is a separate app we need to configure creds. follow the next step
<img width="1566" height="706" alt="image" src="https://github.com/user-attachments/assets/f50e6fb9-df0c-4f08-a573-f893c599d789" />

Manage Jenkins --> Creds --> system --> Global creds -> Add creds -> select secret text
<img width="1620" height="523" alt="image" src="https://github.com/user-attachments/assets/754d26a4-469e-44a3-8b60-613d578332ee" />
<img width="1523" height="737" alt="image" src="https://github.com/user-attachments/assets/781c9eed-d800-40c4-ba05-fbf500f33172" />

Paste that green colour token which we copied
<img width="1489" height="600" alt="image" src="https://github.com/user-attachments/assets/751c772e-1bca-4429-aa21-d5dffef859f2" />
<img width="1475" height="665" alt="image" src="https://github.com/user-attachments/assets/e20698bf-1ac5-4e02-ab35-19debb225216" />

- now add sonarqube in tools and save it
<img width="1580" height="701" alt="image" src="https://github.com/user-attachments/assets/aa4376c9-a276-433b-8768-8a35e31f8da2" />

- we need to mention it under environment, since we can't add this in tools block
<img width="1453" height="666" alt="image" src="https://github.com/user-attachments/assets/1a6749dd-16c4-4e92-ae99-c63e200ac138" />

- sonarqube name come from the System option, there we have configured it
<img width="1041" height="214" alt="image" src="https://github.com/user-attachments/assets/2103a906-2db2-4839-aaaf-956bb2dba20e" />
<img width="1541" height="649" alt="image" src="https://github.com/user-attachments/assets/fecb94cf-a1d7-45d1-98c9-174741263d1c" />

<img width="1509" height="712" alt="image" src="https://github.com/user-attachments/assets/cd35e01e-9811-477d-bcb8-ab6890abc40b" />

Ref:- 
<img width="1667" height="575" alt="image" src="https://github.com/user-attachments/assets/fc0e8b49-1358-4f1e-ad8a-9f36d3db8996" />

- Now build it

<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/fb29a6eb-f523-4435-be9f-e5a7429837f0" />

- once the build is complete , we can see the status of that build

<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/9079231d-c435-4530-b5db-1c7ede114225" />

**Step3: Code Build using Maven**

add mvn clean install, then save it and run the job

<img width="1611" height="689" alt="image" src="https://github.com/user-attachments/assets/a93c9c87-eeb2-4884-bdc5-f6e7d86ae8f9" />

Now go to that particular build no.4 --> workspaces --> we can notice jar file is created

<img width="1587" height="771" alt="image" src="https://github.com/user-attachments/assets/95bfd912-96d3-4bf7-9f8b-df9e91977b21" />

<img width="1461" height="563" alt="image" src="https://github.com/user-attachments/assets/a7be7251-6bba-4b6c-9499-474b028a79b0" />


**Step3: Docker Build**

Prerequisites: 
- Docker should be installed
- since we need to use jenkins with docker, we need to provide permissions ( sudo usermod -aG docker jenkins ), adding jenkins user to docker group
- Now restart jenkins and docker
- Plugins install: Docker Commons, Docker Pipeline, Docker API, Docker
- Manage jenkins --> tools --> Add docker
- Add creds for docker, here we need to use our docker hub password
- Go to pipeline syntax and generate the script for docker, no need to put docker URL, it will automatically pick
- Now add steps for docker in the pipeline script
- Now build it
- Now check in ec2 , image should be there
- Push it into docker hub, create a repository in docker hub so that we can able to push it
- Tag the image and push it
- Now image is ready in docker-hub
  
<img width="1520" height="671" alt="image" src="https://github.com/user-attachments/assets/6f51ac66-8314-4c97-8b63-f2e0aa6e9718" />

<img width="1758" height="970" alt="image" src="https://github.com/user-attachments/assets/bd19813e-0779-45a5-bb7f-d8df360ca5af" />

<img width="1647" height="800" alt="image" src="https://github.com/user-attachments/assets/987842a9-fa72-400f-833e-9d274ce75439" />

<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/26ff3e38-d7ac-427e-aa32-154a9f3712c0" />

<img width="1611" height="554" alt="image" src="https://github.com/user-attachments/assets/4f0a8487-52e3-4cf6-a203-17934b1b8d69" />

<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/777f400f-006e-4e11-8e1f-5c819eecd254" />

<img width="1486" height="597" alt="image" src="https://github.com/user-attachments/assets/c2eeec6e-82b5-4163-a90f-89cec587956b" />

<img width="1335" height="438" alt="image" src="https://github.com/user-attachments/assets/b17f1280-dab6-4ef8-8251-20fab15b78cd" />

<img width="1476" height="795" alt="image" src="https://github.com/user-attachments/assets/39adc9c7-ac3c-46a3-903f-cccb574d7160" />

<img width="1593" height="707" alt="image" src="https://github.com/user-attachments/assets/aa0c513e-8962-4713-90e0-aee9f1e2f0f9" />

<img width="1584" height="799" alt="image" src="https://github.com/user-attachments/assets/684207de-8966-4afc-8307-0031ae88eff2" />

<img width="1604" height="742" alt="image" src="https://github.com/user-attachments/assets/c0b1cf9a-043b-4dc7-b40f-1b04489cdfd3" />

<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/530d3272-462d-4f40-ad71-8d80a2298a51" />


**Step4: Deploy to Kubernetes**

- Install the k8 in ec2 by using kubeadm.sh script, it will be in the devops doc repo
- Now check installed or not
- Plugins: Kubernetes CLI
- Now go to ec2, home location cd ~/.kube, then copy the content from config file and save it in ur lap
- Add creds in jenkins for kubernetes, kind select as secret , then choose the file option and upload your text file
- Now generate a syntax using that cred , no need to select anything other than creds. then click generate pipeline script
- Add the block for k8 in pipeline script
- Edit the image name accordingly in git file
- Now build is successful
- pipeline is successful, we will get error because of code issue

<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/0f9731fb-3928-4582-9602-2481b2d1cec3" />
<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/ed0ee7d1-20aa-459e-b4d8-2916973fa811" />
<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/0cc80060-b653-4548-877e-9b4b962158b6" />
<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/e27060c8-0993-4da4-a0f9-3da940e7c0b9" />
<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/735967a0-76d4-410b-934e-87f5db3ca754" />
<img width="1723" height="752" alt="image" src="https://github.com/user-attachments/assets/b2a25856-443e-4660-bae3-92b30a1badfb" />
<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/9adc528c-405c-4db0-8373-e74a5d3c320c" />
<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/ce60d814-1a6e-4378-8e6a-c86f91340515" />
<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/4ec06a23-b750-45f1-8046-7d8915e04bd4" />
<img width="1414" height="293" alt="image" src="https://github.com/user-attachments/assets/ff828720-e939-42d5-a30a-edab9fa198ac" />
<img width="1519" height="797" alt="image" src="https://github.com/user-attachments/assets/009739f3-966d-4fc4-982f-456c409f980b" />
<img width="1696" height="682" alt="image" src="https://github.com/user-attachments/assets/aa0a31be-83dc-443c-ab42-81998325b44d" />
<img width="1691" height="581" alt="image" src="https://github.com/user-attachments/assets/bea8130b-1b74-4902-8cbb-b641f4b00679" />
<img width="1493" height="637" alt="image" src="https://github.com/user-attachments/assets/9f3b6953-3c69-4794-aa07-9869afa28688" />
<img width="1500" height="342" alt="image" src="https://github.com/user-attachments/assets/32f971bd-40de-4acc-8003-c9492a57eb2a" />


