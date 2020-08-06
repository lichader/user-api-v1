pipeline {
    agent {
        docker { image 'gradle:6.5-jdk11' }
    }
    stages {
        stage('Build') {
            steps {
                sh 'gradle build'
            }
        }
    }
    post {
        always {
            junit 'build/reports/**/*.xml'
        }
    }
}