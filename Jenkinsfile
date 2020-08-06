pipeline {
    agent {
        docker { image 'gradle:6.5-jdk11' }
    }
    stages {
        stage('Test') {
            steps {
                sh 'gradle --version'
            }
        }
    }
}