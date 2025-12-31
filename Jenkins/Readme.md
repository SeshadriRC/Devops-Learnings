- To setup a CI/CD pipeline we need jenkins server
- To install a jenkins java is a prerequisite
- Jenkins should be hosted on separate server. for eg: Jenkins server
- Jenkins run on port 8080


# My learnings
- [Plugins](#plugins)
- [User Creation in Jenkins](#User-creation)
    - [Authorization](#authorization)


# Plugins

- Project based matrix authorization strategy -> useful for user authorization [practice](https://github.com/SeshadriRC/Devops/blob/main/Jenkins/Practicals/Level-1/Day3%3A%20Configure%20Jenkins%20User%20Access.md)
- Folders plugin -> used to create folder in jenkins [practice](https://github.com/SeshadriRC/Devops/blob/main/Jenkins/Practicals/Level-1/Day4:%20Organize%20Jenkins%20Jobs%20with%20Folders.md)

# User Creation

- User management will be found in Manage jenkins -> Security -> Users

  <img width="1588" height="400" alt="image" src="https://github.com/user-attachments/assets/d9093113-bece-48c5-b460-e1292f98180d" />
  <img width="1919" height="498" alt="image" src="https://github.com/user-attachments/assets/526d96d5-2086-4842-b5e3-0cd2e29ac0db" />
  

## Authorization

- If no permissions, user will get like below

 <img width="1919" height="396" alt="image" src="https://github.com/user-attachments/assets/f72597a6-e830-4206-b205-ae86fe051467" />

- It will be found in Jenkins -> Security -> Security

  <img width="1919" height="1000" alt="image" src="https://github.com/user-attachments/assets/3dee772a-80c2-4771-b108-7805c2d8f11d" />
  <img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/5ff990e7-cd05-4de4-9f3f-ebcb08ccb3d0" />

- Configure authorization as per the job

  go to the job -> configure -> enable project based security -> here we can give inherit or doest not inherit

  <img width="1709" height="815" alt="image" src="https://github.com/user-attachments/assets/213abd7f-fd2f-4eb9-950d-43dc77a2093e" />


