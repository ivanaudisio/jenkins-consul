pipeline {
  agent any
  stages {
    stage('Create Apps') {
      parallel {
        stage('App1') {
          steps {
            sh '''mkdir app1
echo "hello app1" > index.html 
echo FROM nginx > Dockerfile
echo COPY index.html /usr/share/nginx/html/index.html
docker build -t app1 .
docker run --name app1 -p 8081:80 -d app1'''
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