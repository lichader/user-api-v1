pipeline {
    environment {
        registryCredential = "dockerhub_id"
        newImage = ""
    }
    agent {
        docker { image 'gradle:6.5-jdk11' }
    }
    stages {
        stage('Build') {
            steps {
                sh 'gradle build'
            }
        }
        stage('Build Image') {
            steps {
                echo 'Start building a docker image'

                script {
                    newImage = docker.build("lichader/user-api-v1:${env.BUILD_ID}")
                    newImage.push()
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
    post {
        success {
            echo "Build success"
        }
        always {
            junit 'build/test-results/**/*.xml'
        }
    }
}