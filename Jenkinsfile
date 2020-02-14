pipeline {
  agent {
    node {
      label 'docker'
    }
  }
  stages {
    stage('Build result') {
      steps {
        sh 'docker build -t furbaez/result:1.0 ./result'
      }
    } 
    stage('Build vote') {
      steps {
        sh 'docker build -t furbaez/vote:1.0 ./vote'
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
          sh 'docker push furbaez/result:1.0'
        }
      }
    }
    stage('Push vote image') {
      when {
        branch 'master'
      }
      steps {
        withDockerRegistry(credentialsId: 'dockerbuildbot-index.docker.io', url:'') {
          sh 'docker push furbaez/vote:1.0'
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
