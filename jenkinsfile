pipeline {
  environment {
    registry = "leotardn/jenkins-pipeline"
    registryCredential = 'dockerhub'
    dockerImage = ''
  }
  agent {dockerfile true }
  stages {
  stage('Test') {
      steps {
        sh 'npm test'
      }
   }
   stage('Building image') {
      steps{
        script {
          dockerImage = docker.build registry + ":$BUILD_NUMBER"
        }
      }
    }
    stage('Deploy Image') {
      steps{
         script {
            docker.withRegistry( '', registryCredential ) {
            dockerImage.push()
          }
        }
      }
    }
    stage('Remove Unused docker image') {
      steps{
        sh "docker rmi $registry:$BUILD_NUMBER"
      }
    }
  }
}
