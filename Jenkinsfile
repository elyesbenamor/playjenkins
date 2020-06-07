pipeline {

  environment {
    registry = "elyes000/test"
    registryCredential = "dockerhub"
    dockerImage = ""
  }

  agent any

  stages {

    stage('Checkout Source') {
      when {
                branch 'master'
            }
      steps {
        git 'https://github.com/elyesbenamor/playjenkins.git'
      }
    }

    stage('Build image') {
      when {
                branch 'master'
            }
      steps{
        script {
          dockerImage = docker.build registry + ":$BUILD_NUMBER"
        }
      }
    }

    stage('Push Image') {
      when {
                branch 'master'
            }
      steps{
        script {
          docker.withRegistry( registry , registryCredential ) {
            dockerImage.push()
          }
        }
      }
    }

    stage('Deploy App') {
      when {
                branch 'master'
            }
      steps {
        script {
          kubernetesDeploy(configs: "myweb.yaml", kubeconfigId: "mykubeconfig")
        }
      }
    }

  }

}
