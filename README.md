# jenkins

> Jenkins repository

---


## installation of jenkins 
---
### To download the setup 
* https://www.jenkins.io/
* download the war file 

## To start 
* java -jar jenkins.war 
* admin password will be in consoel wile running copy that for entering into the app
* open jenkins web console in browser 
	* http://localhost:8080/
	* provide the admin pwd which copied from console earlier 

## install the suggested plugings 
* create admin user 
* user name / user pwd 


## jenkins with git configuration 

* create a spring boot sample project and saved into the git 
* open jenkins web console in browser 
	* http://localhost:8080/
	* provide the user/pwd into the 
* CREATE A NEW JOb
```	
cretae a new job -> ener the item name -> freestyle project -> ok 
```
* GENERAL TAB
```
Genral tab -> select github project -> provide the github url (http://adarshkumarsingh83/jenkins-spring)-> advance button  -> display name =(xxxxx) ->
```
* SOURCE CODE MGMT 
```
source code mgmt-> git -> repository url (http://adarshkumarsingh83/jenkins-spring.git) -> credentials -> click on add -> user/pwd -> select from dropdown-> 
```
* BUILD TRIGGER 
```
build Trigger -> poll scm -> schedule -> * * * * * (for every day every min every seceound scan for changed in github)
```
* BUILD 
```
build -> select -> invok top-level-maven targets -> goal -> install 
```
* POST BUILD ACTION 
```
post build actions -> email notification -> recipients -> xxx@gmail.com 
````
* JENKINS DASHBOARD 
```
go to jenkins home page -> xxx project which we have created recently and -> build now 
```
* clink on apply 

## email configuration 
* jenkins dashboard -> manage jenkins -> configure system -> Email Notification -> 
 ```
Smtp server => smtp.gmail.com
Defualt user email suffix => @gmail.com
Click on SMTP Authentication 
 Username => sender gmail id xxx@gmail.com
 Pwd =>   sender gmail pwd xxx@gmail.com
 SMTP port=> 465 
 Repy to Address => Recipent email id xxx@gmail.com
 Test Configuration by sender test email to test email receipt  => Recipent email id xxx@gmail.com
 Click on test configuration button 
 ```
 * click on save button 

## now on every commit to the git build will trigger and email will trigger 


----

## jenkins docker 

* Docker plugin installation 
* jenkins dashbboard -> manage jenkins -> manage plugins -> avaliables -> search for "docker" in filter -> Cloudbees Docker Build and Publish plugin   
* jenkins dashbboard -> manage jenkins -> manage plugins -> avaliables -> search for "docker" in filter -> docker build step 
* jenkins dashbboard -> manage jenkins -> manage plugins -> avaliables -> search for "docker" in filter ->  docker plugin  
	* to verify Docker dashboard -> project -> configure -> build -> dropdown we can see option for docker build and publish 

## Create spring boot application with docker file to create docker image 
* Dockerfile
```
From openjdk:8
EXPOSE: 9090
ADD target/xxx.jar xxx.jar 
ENTRYPOINT ["java","jar","/xxxx.jar"]
```

* push to the github repository 

### provide dockerhub credentials to the jenkins 
* Docker dashboard -> project -> configure -> add build steps -> Docker build and publish -> 
```
Respository Name      :=> tagname/docker-image-name
TAG                   :=> blank
Docker host url       :=> blank
server credentials    :=> blank
Docker resitory Url   :=> blank
Registory credentials :=> add the dockerhub credentials into this 

```
* apply ->save 


## to start the spring app after build 
* Docker dashboard -> project -> configure ->  ADD post step ->
```
#!/bin/bash
cp /var/lib/jenkins/workspace/xxx/taget/xxx.jar /home/xxx.jar
java -jar xxx.jar 
```

* Jenkinsfile
```
pipeline {
    agent any
    tools {
        maven 'Maven 3.3.9'
        jdk 'jdk8'
    }

    triggers {
        pollSCM '* * * * *'
    }

    stages {
        stage ('Initialize') {
            steps {
                sh '''
                    echo "PATH = ${PATH}"
                    echo "M2_HOME = ${M2_HOME}"
                '''
            }
        }

        stage ('Build') {
            steps {
                sh 'mvn -Dmaven.test.failure.ignore=true install' 
            }
            post {
                success {
                    junit 'target/surefire-reports/**/*.xml' 
                }
            }
        }
    }
}
```


* [help link ](https://www.youtube.com/watch?v=Nw3UohhcPO0&list=PLVz2XdJiJQxwS0BZUHX34ocLTJtRGSQzN&index=1)

* https://www.youtube.com/watch?v=sCcuUMn1vdM&list=PL0UJI1nZ56yC7X07TY7FST6XSOsEJ4Xvy
* https://tomgregory.com/building-a-spring-boot-application-in-jenkins/

```

         stage('Test') { 
             steps { 
                echo "Test" 
                sh 'mvn test' 
            } 
        }  

```


