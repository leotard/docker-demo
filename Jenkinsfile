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
                sh(returnStdout: true, script: '''#!/bin/bash
                    if [ ! "$(docker ps -q -f name=app1)" ]; then
                        if [ "$(docker ps -aq -f status=running -f name=app1)" ]; then
                            # cleanup
                            docker stop app1
                        fi
                        # run your container
                        docker run -p 3000:3000 --rm -d --name app1 $registry:latest
                    fi
                '''.stripIndent())
            }
        }
        
    }
}
