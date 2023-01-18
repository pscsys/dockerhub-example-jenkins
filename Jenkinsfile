pipeline {
  agent { label 'docker' }
  options {
    buildDiscarder(logRotator(numToKeepStr: '5'))
  }
  environment {
    DOCKERHUB_CREDENTIALS = credentials('dockerhubcreds')
  }
  stages {
    stage('Build') {
      steps {
        sh 'docker build -t pescek/dp-alpine:latest .'
      }
    }
    stage('Login') {
      withCredentials([usernamePassword(credentialsId: 'dockerhubcreds', passwordVariable: '', usernameVariable: '')]) {
          sh 'echo $passwordVariable | docker login -u $usernameVariable --password-stdin'
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
