pipeline {
    agent any

    environment {
        IMAGE_NAME = "het-devops-web"
    }

    stages {

        stage('Checkout') {
            steps {
                checkout scm
            }
        }

        stage('Build Docker Image') {
            steps {
                sh 'docker build -t ${IMAGE_NAME}:${BUILD_NUMBER} .'
            }
        }

        stage('Stop Old Container') {
            steps {
                sh 'docker rm -f het-devops-web || true'
            }
        }

        stage('Deploy') {
            steps {
                sh 'docker run -d --name het-devops-web -p 8085:80 ${IMAGE_NAME}:${BUILD_NUMBER}'
            }
        }

        stage('Verify') {
            steps {
                sh 'sleep 3'
                sh 'curl -f http://localhost:8085'
            }
        }
    }

    post {
        success {
            echo "🚀 Deployment Successful - Image: ${IMAGE_NAME}:${BUILD_NUMBER}"
        }

        failure {
            echo '❌ Deployment Failed!'
        }
    }
}
