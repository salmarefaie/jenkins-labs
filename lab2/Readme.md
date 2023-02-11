### 1- configure jenkins image to run docker commands on your hos docker daemon
 - build Dockerfile
  ```bash 
       docker build -t jenkins-image .
       docker run -it -d -p 8084:8080 -v /var/run/docker.sock:/var/run/docker.sock jenkins-image
       docker container ps -a
       docker exec -it f2302c803913 bash
       docker ps -a 
  ```
 ![Screenshot from 2023-02-11 02-51-18](https://user-images.githubusercontent.com/76884936/218252050-8d6cc060-d730-48f8-9d90-acf961585c45.png)

### 2- create CI/CD for this repo https://github.com/mahmoud254/jenkins_nodejs_example.
  - from manage jenkins, choose manage credintials
  - add docker hub account credintials 
  - from new item, make pipeline and write pipeline script
  ``` bash
  pipeline {
    agent any

    stages {
        stage('CI') {
            steps {
                withCredentials([usernamePassword(credentialsId: 'docker-hub', usernameVariable: 'USERNAME', passwordVariable: 'PASSWORD')]) {
                git 'https://github.com/mahmoud254/jenkins_nodejs_example'
                sh """
                docker login -u ${USERNAME} -p ${PASSWORD}
                docker build . -f dockerfile -t salmarefaie29/node-app:v1.0
                docker push salmarefaie29/node-app:v1.0
                """
                }
            }
        }
         stage('CD') {
            steps {
                withCredentials([usernamePassword(credentialsId: 'docker-hub', usernameVariable: 'USERNAME', passwordVariable: 'PASSWORD')]) {
                git 'https://github.com/mahmoud254/jenkins_nodejs_example'
                sh """
                docker login -u ${USERNAME} -p ${PASSWORD}
                docker run -d -p 3000:3000 salmarefaie29/node-app:v1.0
                """
                }
            }
        }
    }
}
  ```
 ![Screenshot from 2023-02-11 12-42-43](https://user-images.githubusercontent.com/76884936/218267339-489bd8cf-cc04-4d62-97d5-076989888e29.png)
 
 ![Screenshot from 2023-02-11 12-41-42](https://user-images.githubusercontent.com/76884936/218267350-9873418e-78db-49c7-9214-abc48ad17413.png)
 
 ![Screenshot from 2023-02-11 12-43-07](https://user-images.githubusercontent.com/76884936/218267387-af3a0239-6e0c-4f02-ad08-27a0013b02ca.png)
 
 ![Screenshot from 2023-02-11 12-43-28](https://user-images.githubusercontent.com/76884936/218267461-2275791d-6588-4133-b332-ad5e47b1769e.png)
 
 
 ### 3- create docker file to build image for jenkins slave
   ```bash 
       docker build -t jenkins-slave .
       docker images
  ```
  ![Screenshot from 2023-02-11 18-34-36](https://user-images.githubusercontent.com/76884936/218269716-eaece6d4-c6b7-4477-9d2f-43b07c281163.png)

### 4- create container from this image and configure ssh 
```bash
     docker run -it -d --name jenkins-slave-container jenkins-slave 
     docker exec -it 7f2302c803913 bash
     ssh-keygen
     cd /root/.ssh
     ls
     cat id_rsa ... to copy it to id_rsa for jenkins_slave container
     cat id_rsa.pub  ... to copy it to id_rsa.pub for jenkins_slave container
     exit
     docker exec -it 75c0fecb5f3b  bash
     cd /root/.ssh
     apt install vim
     vim id_rsa
     vim id_rsa.pub
     cp id_rsa.pub authorized-keys 
     exit   
```

### 5- from jenkins maste create new node with the slave container
 - choose manage jenkins, then choose manage nodes and clouds and create new node

![Screenshot from 2023-02-11 18-21-12](https://user-images.githubusercontent.com/76884936/218270270-7474eee8-9ab8-4e1e-b6ed-8b32c5e587a4.png)

![Screenshot from 2023-02-11 18-21-24](https://user-images.githubusercontent.com/76884936/218270279-32c27db9-d154-4386-b85d-db297458bc1c.png)

### 6- integrate slack with jenkins

![Screenshot from 2023-02-11 13-20-32](https://user-images.githubusercontent.com/76884936/218270540-dc92ba05-bf4b-4bd9-8507-bd49e5303ec9.png)

### 7- send slack message when stage in your pipeline is successful
- choose manage jenkins, then choose manage plugins and install slack notification plugin
- create new pipeline and write pipeline script
```bash
   pipeline { agent any stages { stage('Hello') { steps { echo 'Hello World' }
      post{ success{ slackSend(message:"build done successfully , here is the url ${BUILD_URL}") }
      failure{ slackSend(message:"buid failed , here is the url ${BUILD_URL}" ) } } } } }
```

![Screenshot from 2023-02-11 13-23-00](https://user-images.githubusercontent.com/76884936/218270582-e6affbd3-2583-4e02-8b7f-f86d821c71cf.png)

### 8- install audit logs plugin and test it
- choose manage jenkins, then choose manage plugins and audit log plugin

![Screenshot from 2023-02-11 12-26-24](https://user-images.githubusercontent.com/76884936/218270693-cd9d03e9-7316-4760-88af-25c434ca962d.png)

![Screenshot from 2023-02-11 12-26-34](https://user-images.githubusercontent.com/76884936/218271805-6b66189e-f332-438f-a181-4a4fd0ad709c.png)

### 9-  fork the following repo https://github.com/mahmoud254/Booster_CI_CD_Project and add dockerfile to run this django app and use github actions to build the docker image and push it to your dockerhub
- Dockerfile
```bash 
  FROM ubuntu
  RUN apt update -y
  RUN apt-get install python3 -y
  RUN apt install pip -y
  COPY . .
  RUN pip3 install -r requirements.txt
  RUN python3 manage.py makemigrations
  RUN python3 manage.py migrate
  EXPOSE 8000
  CMD ["python3","manage.py","runserver 0.0.0.0:8000"]
```
- Docker-compose.yaml
```bash 
    name: Docker Image CI
    on:
    push:
    branches: [ "master" ]
    pull_request:
    branches: [ "master" ]
    jobs:
    build:
    runs-on: ubuntu-latest
    steps:
    - uses: actions/checkout@v3
    - name: Build the Docker image
    run: |
    docker build . -f Dockerfile -t salmarefaie29/django-app:v1.0
    docker login -u ${{secrets.DOCKER_USERNAME}} -p ${{secrets.DOCKER_PASSWORD}}
    docker push salmarefaie29/django-app:v1.0 
```

![Screenshot from 2023-02-11 12-49-57](https://user-images.githubusercontent.com/76884936/218272501-56bb5b60-4903-4588-8236-b86dd598048d.png)

![Screenshot from 2023-02-11 12-50-20](https://user-images.githubusercontent.com/76884936/218272505-c21fa76f-0a67-4ed5-9d20-ce84b36f56ec.png)

![Screenshot from 2023-02-11 13-13-57](https://user-images.githubusercontent.com/76884936/218272528-dff241f6-d4a1-45da-980e-100a7c6d426d.png)

