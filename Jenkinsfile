pipeline {
    agent any

    stages {

        stage('Clone Complete') {
            steps {
                echo 'Repository downloaded successfully.'
            }
        }

        stage('Build Docker Image') {
            steps {
                sh 'docker build -t project3-demo .'
            }
        }

        stage('Show Docker Images') {
            steps {
                sh 'docker images'
            }
        }

    }
}
