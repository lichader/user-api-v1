pipeline {
    agent any
    environment {
        registryCredential = "dockerhub_id"
        newImage = ""
    }
    stages {
        stage('Build') {
            agent {
                docker { image 'gradle:6.5-jdk11' }
            }
            steps {
                sh 'gradle build'
            }
            post {
                always {
                    junit 'build/test-results/**/*.xml'
                }
            }
        }
        stage('Build Image') {
            steps {
                echo 'Start building a docker image'

                script {
                    newImage = docker.build("lichader/user-api-v1:${env.BUILD_ID}")
                }
            }
        }
        stage('Push to Docker hub'){
            steps {
                echo 'Pushing image to docker hub'
                script {
                    docker.withRegistry( '', registryCredential ) { 
                        newImage.push() 
                    }
                }
            }
        }
    }

}