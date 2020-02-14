pipeline {
  agent {
    node {
      label 'docker'
    }
  }
  stages {
    stage('Build result') {
      steps {
        sh 'docker build -t furbaez/result ./result'
      }
    } 
    stage('Build vote') {
      steps {
        sh 'docker build -t furbaez/vote ./vote'
      }
    }
    stage('Build worker') {
      steps {
        sh 'docker build -t furbaez/worker:1.0 ./worker'
      }
    }
    stage('Push result image') {
      when {
        branch 'master'
      }
      steps {
        withDockerRegistry(credentialsId: 'dockerbuildbot-index.docker.io', url:'') {
          sh 'docker push furbaez/result'
        }
      }
    }
    stage('Push vote image') {
      when {
        branch 'master'
      }
      steps {
        withDockerRegistry(credentialsId: 'dockerbuildbot-index.docker.io', url:'') {
          sh 'docker push furbaez/vote'
        }
      }
    }
    stage('Push worker image') {
      when {
        branch 'master'
      }
      steps {
        withDockerRegistry(credentialsId: 'dockerbuildbot-index.docker.io', url:'') {
          sh 'docker push furbaez/worker:1.0'
        }
      }
    }
    stage('Deploy') {
      when {
        branch 'master'
      }
      input{
         message "Do you want to proceed for production deployment?"
      }
      steps {
        sh 'docker run -d  furbaez/vote'
      }
    }
  }
}
