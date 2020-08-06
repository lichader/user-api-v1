pipeline {
    agent none
    stages {
        stage('Build') {
            agent {
                docker { image 'gradle:6.5-jdk11' }
            }
            steps {
                sh 'gradle clean build'
            }
            post {
                always {
                    junit 'build/test-results/**/*.xml'
                    sh 'ls build/libs/*'
                }
            }
        }
        stage('Build and push image') {
            agent any
            steps {
                echo 'Start building a docker image'
                sh 'ls build/libs/*'
                script {
                    def newImage = docker.build("lichader/user-api-v1:${env.BUILD_ID}")
                    docker.withRegistry( '', "dockerhub_id" ) { 
                        newImage.push() 
                    }
                }
            }
        }

    }

}