pipeline {
    agent none
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
        }
        stage('Build Image') {
            agent any
            steps {
                echo 'Start building a docker image'

                script {
                    newImage = docker.build("lichader/user-api-v1:${env.BUILD_ID}")
                    newImage.push()
                }
            }
        }
        stage('Push to Docker hub'){
            agent any
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
    post {
        success {
            echo "Build success"
        }
        always {
            junit 'build/test-results/**/*.xml'
        }
    }
}