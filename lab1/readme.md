### 1- install jenkins with docker image
> - sudo docker run -p 8083:8080 -d jenkins/jenkins:lts
> - to know autherized Admin passwort at first time :
>     - sudo docker exec -it 6d34b967f71 bash
>     - cat /var/jenkins_home/secrets/initialAdminPassword
>  - OR
>     - sudo docker logs 6d34b967f71 
> 
> ![Screenshot from 2023-02-04 15-40-52](https://user-images.githubusercontent.com/76884936/216771006-31a435ab-5069-4d1f-aba6-4216a42a7fee.png)

### 2- install role based authorization plugin
> - from manage jenkins section, choose plugin manager
> - from available plugins, we will search about role based authorization plugin and install it
> - after it, we will restart jenkins and start container again
>
> ![Screenshot from 2023-02-04 15-48-55](https://user-images.githubusercontent.com/76884936/216771302-4ffefef4-aec2-4cc8-875e-4f946f26c1fa.png)

### 3- create new user
> - from manage jenkins section, choose manage users, then create user
>
> ![Screenshot from 2023-02-09 00-15-22](https://user-images.githubusercontent.com/76884936/218250186-8ca03bb5-198f-4ec7-926e-0cdfc8d2eb31.png)

### 4- create read role and assign it to the new user
> - from manage jenkins section, choose manage and assign roles
> - create role from manage roles section 
> - from assign roles section, assign role to user 
>
> ![Screenshot from 2023-02-09 00-30-44](https://user-images.githubusercontent.com/76884936/218250751-997eea90-6467-4b2a-be73-fcedc1d3b6ee.png)
>
> ![Screenshot from 2023-02-09 00-33-07](https://user-images.githubusercontent.com/76884936/218251247-5b9105d7-b300-4060-b2d8-47d40db434b5.png)

### 5- create free style pipeline and link it to private git repo(inside it create directory and create file with "hello world")
>- choose new item
>- choose freestyle project 
> 
> ![Screenshot from 2023-02-09 01-02-07](https://user-images.githubusercontent.com/76884936/218251542-dad3ebf5-972c-4562-a0c1-f661b7c9a6fd.png)
>
>![Screenshot from 2023-02-09 01-02-50](https://user-images.githubusercontent.com/76884936/218251548-5a917408-a855-43f8-a481-b606556f4c8a.png)
>
> ![Screenshot from 2023-02-09 01-03-05](https://user-images.githubusercontent.com/76884936/218251565-5fcfb68e-d0ae-4bc7-85f6-46d8756e67df.png)

### 6- create declarative in jenkins GUI pipeline for your own repo to do "ls"
>- choose new item
>- choose pipeline
>
> ![Screenshot from 2023-02-09 01-11-26](https://user-images.githubusercontent.com/76884936/218251605-492f34c0-10ec-413b-b9e0-88bc688308e3.png)
> 
> ![Screenshot from 2023-02-09 01-12-20](https://user-images.githubusercontent.com/76884936/218251617-bf5cbddf-1367-495e-905c-9ab89892d9bd.png)

### 7- create scripted in jenkins GUI pipeline for your own repo to do "ls"
>- choose new item
>- choose pipeline
>
> ![Screenshot from 2023-02-09 01-19-09](https://user-images.githubusercontent.com/76884936/218251686-3a175eb4-fc5e-4f7b-a807-3dea1c2650da.png)
>
> ![Screenshot from 2023-02-09 01-19-33](https://user-images.githubusercontent.com/76884936/218251705-65fec810-7cd8-4a47-b43e-e77338afac58.png)

### 8- create the same with jenkinsfile in your branches as multibranch pipeline
>- choose new item
>- choose multibranch pipeline
>
> ![Screenshot from 2023-02-11 11-54-21](https://user-images.githubusercontent.com/76884936/218251853-271fef7d-3b6f-415f-a3f2-76fa442c2705.png)
> 
> ![Screenshot from 2023-02-11 11-55-06](https://user-images.githubusercontent.com/76884936/218251856-d5a40b8b-8170-4dbe-8f14-082a17fe4f11.png)
>
>![Screenshot from 2023-02-11 11-55-22](https://user-images.githubusercontent.com/76884936/218251868-3f5814bf-7b52-4c27-b4e8-35ee6c439465.png)


