pipeline {
    agent any

    environment {
        IMAGE_NAME = "bhukyakeerthana/node-app"
        IMAGE_TAG = "${BUILD_NUMBER}"
    }

    stages {

        stage('Build Image') {
            steps {
                bat "docker build -t %IMAGE_NAME%:%IMAGE_TAG% ."
            }
        }

        stage('Push Image') {
            steps {
                withDockerRegistry(credentialsId: 'docker-hub-creds', url: '') {
                    bat "docker push %IMAGE_NAME%:%IMAGE_TAG%"
                }
            }
        }

        stage('Deploy to Kubernetes') {
            steps {
                withCredentials([file(credentialsId: 'kubeconfig', variable: 'KUBECONFIG')]) {
                    bat "kubectl set image deployment/node-app-deployment node-app=%IMAGE_NAME%:%IMAGE_TAG%"
                }
            }
        }
    }
}