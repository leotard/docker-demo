pipeline {
  environment {
    registry = "leotardn/jenkins-pipeline"
    registryCredential = 'dockerhub'
    dockerImage = ''
  }
  agent {
  dockerfile {
        filename 'Dockerfile'
        registryUrl 'https://hub.docker.com/repository/docker/leotardn/jenkins-pipeline'
        registryCredentialsId 'dockerhub'
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
