pipeline {
  agent {
    docker { image 'node:16-alpine' }
  }
  options {
    buildDiscarder(logRotator(numToKeepStr: '5'))
  }
  environment {
    DOCKERHUB_CREDENTIALS = credentials('pescek')
  }
  stages {
    stage('Build') {
      steps {
        sh 'docker build -t pescek/dp-alpine:latest .'
      }
    }
    stage('Login') {
      steps {
        sh 'echo $DOCKERHUB_CREDENTIALS_PSW | docker login -u $DOCKERHUB_CREDENTIALS_USR --password-stdin'
      }
    }
    stage('Push') {
      steps {
        sh 'docker push pescek/dp-alpine:latest'
      }
    }
  }
  post {
    always {
      sh 'docker logout'
    }
  }
}
