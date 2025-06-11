# SonarQube Setup
## Launch EC2 instane
- launch EC2 instance with following configuration:
   - OS : ubuntu
   - instance type : t3.medium
   - security group : allow following ports
       -   port no :22 (SSH)
       -   port no : 8080 (jenkins)
       -   port no : 9000 (sonarqube)
       -   storage : 30 GB
- Connect the instance.
## install Jenkins
```
sudo apt update
sudo apt install fontconfig openjdk-21-jre  -y
sudo wget -O /etc/apt/keyrings/jenkins-keyring.asc \
  https://pkg.jenkins.io/debian-stable/jenkins.io-2023.key
echo "deb [signed-by=/etc/apt/keyrings/jenkins-keyring.asc]" \
  https://pkg.jenkins.io/debian-stable binary/ | sudo tee \
  /etc/apt/sources.list.d/jenkins.list > /dev/null
sudo apt-get update
sudo apt-get install jenkins -y
```
- unlock jenkins (copy instance IP address and paste on browser with port number 8080, eg. ``13.48.178.78:8080``.
- copy the directory location to get the Administrator password.
- go back to instance and run the command.
```
sudo cat /var/lib/jenkins/secrets/initialAdminPassword
```
- copy the password and paste it on jenkins browser.
![Screenshot 2025-06-11 135658](https://github.com/user-attachments/assets/9ebc0a3d-4348-4245-be36-4041842c7d1c)
- now install suggested plugins.
- log in to jenkins.
## install Docker
- now install docker using the following command
```

sudo apt-get update
sudo apt-get install docker.io -y
sudo systemctl start docker
sudo usermod -aG docker ubuntu
newgrp docker
sudo chmod 777 /var/run/docker.sock
```
## Install SonarQube
- run the following command to install SonarQube.
- it will install SonarQube as a container.
```
docker run -d --name sonar -p 9000:9000 sonarqube:lts-community
```
![Screenshot 2025-06-11 140838](https://github.com/user-attachments/assets/dcf2df2b-7f0e-4f9d-9494-ed9860d850dd)

- now, copy instance IP address and paste on browser with port number 9000, eg. ``13.48.178.78:9000``.
- following page will open.
![Screenshot 2025-06-11 141233](https://github.com/user-attachments/assets/f742c224-81b6-4ed8-8650-fc2d67a6d39b)
    - by Default name : admin
    - password : admin.
- log in to sonarQube and change the password.
![Screenshot 2025-06-11 141624](https://github.com/user-attachments/assets/70150252-ac9f-4573-9fd3-0c83cd9d0a3f)
- go to admin, select "my account", select "security" and generate token.
![Screenshot 2025-06-11 142401](https://github.com/user-attachments/assets/3c9ab6ce-9dab-4785-80be-fae5582bedad)
- Make sure you copy it now, you won't be able to see it again!
- In Jenkins, click on "manage Jenkins" -> go to "Credentials" -> select "global" -> add sonarQube credentials .
    - kind : Secret text
    - secret : paste the sonarqube token
    - Id: sonar-token
    - description: sonar-token
- creat token
## install required plugins
- go to manage Jenkins -> click on "plugins" -> select "available plugins".
- install following plugins.
```
Eclipse Temurin Installer
```
```
SonarQube Scanner
```
```
NodeJs Plugin
```
```
stage view
```
```
Docker
```
```
Docker API
```
```
Docker Pipeline
```
```
Docker Commons
```
## Tools configuration
- go to manage Jenkins -> select "tools" 
-  add JDK
  - name: jdk17
  - install automatically
  - version : select jdk -17.0.11 +9 
- add SonarQube-scanner
  -  name: sonr-scanner
  -  install automatically
  -  version: default version.
- add Nodejs
  - name: node16
  - install automatically
  - version : NodeJS 16.10.0
- add Docker
  -  name: docker
  -  install automatically
  -  installer : Download from docker.com
- apply and save
## Configure SonarQube Server
- go to manage Jenkins -> select "system" -> go to SonarQube server
  - name: sonar-server
  - server URL : URL of our SonarQube page ( eg. ``http://13.48.178.78:9000`` )
  - server authentication token : ( sonarqube credential )
- apply and save
![Screenshot 2025-06-11 150426](https://github.com/user-attachments/assets/dae34016-948d-4729-9d52-816d145bb42a)
## Pipeline creation
- click on new item
  - name :Jenkins-sonar-integration
  - type: pipeline 

- **pipeline 1**
```
pipeline {
    agent any
    tools {
        jdk 'jdk17'
        nodejs 'node16'
    }
    environment {
        SCANNER_HOME = tool 'sonar-scanner'
    }
    stages {

        stage('Code-Pull') {
            steps {
                git branch: 'main', url: 'https://github.com/abhipraydhoble//Project-Netflix-Clone.git'
            }
        }
        stage("Sonarqube Analysis") {
            steps {
                withSonarQubeEnv('sonar-server') {
                    sh '''$SCANNER_HOME/bin/sonar-scanner -Dsonar.projectName=Netflix \
                    -Dsonar.projectKey=Netflix'''
                }
            }
        }
        stage("quality gate") {
            steps {
                script {
                    waitForQualityGate abortPipeline: false, credentialsId: 'sonar-token'
                }
            }
        }
        stage('Install Dependencies') {
            steps {
                sh "npm install"
            }
        }
        stage('Deploy'){
            steps{
                 sh "docker build --build-arg TMDB_V3_API_KEY=020581a34f3ab93b1360a55bea864bd9 -t netflix ."
                 sh "docker run -itd --name netflix -p 8081:80 netflix"
            }
        }
        
    }
}
```
- apply and save
- run the following commands to give permission to user to create  the containers
```
sudo usermod -aG docker jenkins
newgrp docker
sudo chmod 777 /var/run/docker.sock
```
- now build the pipelie 1 by clicking on "build now"


