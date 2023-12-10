pipeline {
  agent any
  

  stages {

    stage("Clean Ups"){
        steps{
            sh 'docker-compose down'
        }
    }

    stage('Build') {
      steps {
        sh 'docker-compose build --no-cache'
      }
    }



    stage('Push Images') {
      steps {
        withCredentials([usernamePassword(credentialsId: "${DOCKER_REGISTRY_CREDS}", passwordVariable: 'DOCKER_PASSWORD', usernameVariable: 'DOCKER_USERNAME')]) {
          sh "echo \$DOCKER_PASSWORD | docker login -u \$DOCKER_USERNAME --password-stdin docker.io"
          sh 'docker-compose push'
        }
      }
    }

    stage("Deploy"){

        steps{
            sh 'docker-compose up -d'
        }

    }



  }



}
