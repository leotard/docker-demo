pipeline {
  environment {
    registry = "leotardn/jenkins-pipeline"
    registryCredential = 'dockerhub'
    dockerImage = ''
  }
  agent {
  dockerfile {
        filename 'Dockerfile'
        registryCredentialsId 'dockerhub'
        args '-v /var/run/docker.sock:/var/run/docker.sock'
    }
  }
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
