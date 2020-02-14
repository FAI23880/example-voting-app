pipeline {
  agent {
    node {
      label 'ubuntu-1604-aufs-stable'
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
        sh 'docker build -t furbaez/worker ./worker'
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
          sh 'docker push furbaez/worker'
        }
      }
    }
  }
}
