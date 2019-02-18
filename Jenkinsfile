pipeline {
  agent any
  stages {
    stage('Create Apps') {
      parallel {
        stage('App1') {
          environment {
            APP_NAME = 'app1'
          }
          steps {
            git 'https://github.com/stormpath/stormpath-spring-boot-war-example.git'
            sh 'mvn install'
            sh '''mv target/*.war target/$APP_NAME.war
echo From tomcat:8-jre8 > Dockerfile
echo COPY target /usr/local/tomcat/webapps/ >> Dockerfile
docker build -t $APP_NAME .'''
            sh 'docker run -d -p 8081:8080 --name $APP_NAME --network jenkins_js-network $APP_NAME'
          }
        }
        stage('App2') {
          steps {
            sh '''mkdir app2
echo "hello app2" > app2/index.html 
docker run --name app2 -p 8082:8080 -v /app2:/usr/share/nginx/html:ro -d nginx'''
          }
        }
      }
    }
    stage('App1') {
      parallel {
        stage('App1') {
          steps {
            sh 'echo hello'
          }
        }
        stage('App2') {
          steps {
            sh 'echo hello'
          }
        }
      }
    }
    stage('Destroy') {
      steps {
        sh 'echo hello'
      }
    }
  }
}