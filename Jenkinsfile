pipeline {
  environment {
     def VERSION = "1.6"
  }
  agent {
    node {
      label 'manager'
    }
  }
  stages {
    stage('Build result') {
      steps {
        sh "docker build -t furbaez/result:${env.VERSION} ./result"
      }
    } 
    stage('Build vote') {
      steps {
        sh "docker build -t furbaez/vote:${env.VERSION} ./vote"
      }
    }
    stage('Build worker') {
      steps {
        sh "docker build -t furbaez/worker:${env.VERSION} ./worker"
      }
    }
    stage('Push result image') {
      when {
        branch 'master'
      }
      steps {
        withDockerRegistry(credentialsId: 'dockerbuildbot-index.docker.io', url:'') {
          sh "docker push furbaez/result"
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
          sh 'docker push furbaez/worker'
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
        sh 'docker stack deploy --compose-file docker-stack.yml vote'
      }
    }
  }
}
