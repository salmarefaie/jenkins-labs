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
