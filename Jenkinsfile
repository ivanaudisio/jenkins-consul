pipeline {
  agent any
  stages {
    stage('Build Apps') {
      parallel {
        stage('App1') {
          agent {
            docker {
              image 'maven:3-jdk-7-slim'
            }

          }
          environment {
            APP_NAME = 'app1'
          }
          steps {
            git 'https://github.com/stormpath/stormpath-spring-boot-war-example.git'
            sh 'mvn install'
            stash(name: 'warfile1', includes: 'target/*.war')
          }
        }
        stage('App2') {
          agent {
            docker {
              image 'maven:3-jdk-7-slim'
            }

          }
          environment {
            APP_NAME = 'app2'
          }
          steps {
            git 'https://github.com/stormpath/stormpath-spring-boot-war-example.git'
            sh 'mvn install'
            stash(name: 'warfile2', includes: 'target/*.war')
          }
        }
      }
    }
    stage('Run Containers') {
      parallel {
        stage('App1') {
          steps {
            unstash 'warfile1'
            sh '''mv target/*.war target/app1.war
echo From tomcat:8-jre8 > Dockerfile
echo COPY target /usr/local/tomcat/webapps/ >> Dockerfile
docker build -t app1 .
docker run -d -p 8081:8080 --name app1 --network jenkins_js-network app1
'''
          }
        }
        stage('App2') {
          steps {
            unstash 'warfile2'
            sh '''mv target/*.war target/app2.war
echo From tomcat:8-jre8 > Dockerfile
echo COPY target /usr/local/tomcat/webapps/ >> Dockerfile
docker build -t app2 .
docker run -d -p 8082:8080 --name app2 --network jenkins_js-network app2
'''
          }
        }
      }
    }
    stage('Test Apps') {
      parallel {
        stage('App1') {
          steps {
            sh '''sleep 10
response=$(curl -s -o /dev/null -w "%{http_code}\\\\\\\\n" app1:8080)
if ["$response" != "200"]
then
exit 1
fi 
'''
          }
        }
        stage('App2') {
          steps {
            sh '''sleep 10
response=$(curl -s -o /dev/null -w "%{http_code}\\\\\\\\n" app2:8080)
if ["$response" != "200"]
then
exit 1
fi 
'''
          }
        }
      }
    }
  }
  environment {
    APP_PORT = '8081'
  }
}