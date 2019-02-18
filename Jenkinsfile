pipeline {
  agent any
  stages {
    stage('Create Apps') {
      parallel {
        stage('App1') {
          steps {
            sh '''echo "hello app1" > app1/index.html 
docker run --name app1 -v /app1:/usr/share/nginx/html:ro -d nginx'''
          }
        }
        stage('App2') {
          steps {
            sh '''echo "hello app2" > app2/index.html 
docker run --name app2 -v /app2:/usr/share/nginx/html:ro -d nginx'''
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