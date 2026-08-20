pipeline {
    agent any

    stages {

        stage('Checkout') {
            steps {
                checkout scm
            }
        }

        stage('Build Docker Image') {
            steps {
                sh 'docker build -t het-devops-web:v2 .'
            }
        }

        stage('Stop Old Container') {
            steps {
                sh 'docker rm -f het-devops-web || true'
            }
        }

        stage('Deploy') {
            steps {
                sh 'docker run -d --name het-devops-web -p 8085:80 het-devops-web:v2'
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
            echo '🚀 CI/CD Deployment Successful!'
        }

        failure {
            echo '❌ CI/CD Pipeline Failed!'
        }
    }
}
