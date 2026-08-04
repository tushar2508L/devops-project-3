pipeline {
    agent any

    environment {
        IMAGE_NAME = "tushar25l/devops-project-3:${BUILD_NUMBER}"
        K8S_SERVER = "3.110.166.151"
    }

    stages {

        stage('Build Docker Image') {
            steps {
                sh 'docker build -t $IMAGE_NAME .'
            }
        }

        stage('Docker Hub Login') {
            steps {
                withCredentials([usernamePassword(
                    credentialsId: 'dockerhub-ssh-key',
                    usernameVariable: 'DOCKER_USER',
                    passwordVariable: 'DOCKER_PASS'
                )]) {
                    sh 'echo $DOCKER_PASS | docker login -u $DOCKER_USER --password-stdin'
                }
            }
        }

        stage('Push Docker Image') {
            steps {
                sh 'docker push $IMAGE_NAME'
            }
        }

        stage('Deploy to Kubernetes') {
            steps {
                sshagent(credentials: ['kubernetes-ssh']) {
                    sh """
                    ssh -o StrictHostKeyChecking=no ubuntu@${K8S_SERVER} '
                    kubectl set image deployment/project4 project4=${IMAGE_NAME} -n project4
                    kubectl rollout status deployment/project4 -n project4
                    '
                    """
                }
            }
        }
    }
}
