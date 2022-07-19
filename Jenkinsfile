pipeline {
    environment {
        registry = "leotardn/jenkins-pipeline"
        registryCredential = 'dockerhub'
        dockerImage = ''
    }
    agent none
    stages {
        stage ('Test'){
            agent {
                dockerfile {
                    filename 'Dockerfile'
                    registryCredentialsId 'dockerhub'
                    args '-p 3000:3000'
                }
            }
            steps {
                sh 'npm test'
            }
            
            
        }
        stage('Building image') {
            agent {label 'jenkins-slave-1'}
            steps{
                script {
                dockerImage = docker.build registry + ":$BUILD_NUMBER"
                }
            }
        }
        stage('Deploy Image') {
            agent {label 'jenkins-slave-1'}
            steps{
                script {
                    docker.withRegistry( '', registryCredential ) {
                        dockerImage.push()
                        dockerImage.push('latest')
                    }
                }
            }
        }
        stage('Remove Unused docker image') {
            agent {label 'jenkins-slave-1'}
            steps{
                dockerImage.withRun('-p 3000:3000'){}
            }
        }
        
    }
}
