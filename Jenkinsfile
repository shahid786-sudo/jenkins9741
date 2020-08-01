pipeline {
  environment {
    registry = "shahid9741/jenkins"
    registryCredential = 'docker-hub'
    testapp = ''
    }
  agent any
  stages {
    stage ('Checkout code') {
      steps{
        checkout scm
      }
    }
    stage ('Building image') {
      steps {
        script {
          testapp = docker.build registry + ":$BUILD_NUMBER"
          }
      }
    }
    stage ("Push") {
      steps {
        script {
          docker.withRegistry('https://registry.hub.docker.com', 'docker-hub') {
            testapp.push("latest")
            testapp.push("$BUILD_NUMBER")
          }
        }
      }
    }
    stage ('Deployment') {
      steps {
        sshagent(['Demo-machine']){
          sh 'sudo ssh -vvv -o StrictHostKeyChecking=no -T shahid@192.168.0.7'
          sh 'sudo scp -r deployment.yaml shahid@192.168.0.7:/home/shahid'
          sh 'sudo chown shahid:shahid deployment.yaml'
          sh 'cd $HOME'
          sh 'sudo kubectl create -f deployment.yaml'
          }
        }
      }
    }
 }
