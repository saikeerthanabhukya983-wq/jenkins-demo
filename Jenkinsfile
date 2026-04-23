pipeline {
    agent any

    environment {
        DOCKER_IMAGE = "bhukyakeerthana/node-app"
        TAG = "${BUILD_NUMBER}"
    }

    stages {

        stage('Checkout Code') {
            steps {
                git branch: 'main', url: 'https://github.com/saikeerthanabhukya983-wq/jenkins-demo.git'
            }
        }

        stage('Build Docker Image') {
            steps {
                bat "docker build -t %DOCKER_IMAGE%:%TAG% ."
            }
        }

        stage('Login to DockerHub') {
            steps {
                withCredentials([usernamePassword(credentialsId: 'docker-hub-creds', usernameVariable: 'USER', passwordVariable: 'PASS')]) {
                    bat "docker login -u %USER% -p %PASS%"
                }
            }
        }

        stage('Push Image') {
            steps {
                bat "docker push %DOCKER_IMAGE%:%TAG%"
            }
        }

        stage('Deploy to Kubernetes') {
            steps {
                withCredentials([file(credentialsId: 'kubeconfig', variable: 'KUBECONFIG')]) {
                    bat """
                    C:\\Users\\KEERTHANA\\Downloads\\helm-v3.14.0-windows-amd64\\windows-amd64\\helm.exe upgrade --install node-app ./node-app-chart ^
                    --set image.repository=bhukyakeerthana/node-app ^
                    --set image.tag=%BUILD_NUMBER%
                    """
                }
            }
        }
    }
}