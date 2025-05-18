pipeline {
    agent any

    environment {
        DOCKER_IMAGE_BACKEND = 'tcms-backend'
        DOCKER_IMAGE_FRONTEND = 'tcms-frontend'
    }

    stages {
        stage('Clone Repo') {
            steps {
                git 'https://github.com/navee-hans/tcms-tool.git'
            }
        }

        stage('Build Docker Images') {
            steps {
                script {
                    sh 'docker build -t $DOCKER_IMAGE_BACKEND ./backend'
                    sh 'docker build -t $DOCKER_IMAGE_FRONTEND ./frontend'
                }
            }
        }

        stage('Start Services') {
            steps {
                sh 'docker-compose up -d'
            }
        }

        stage('Run Tests') {
            steps {
                sh 'docker exec tcms-backend npm test || true'
                sh 'docker exec tcms-frontend npm test || true'
            }
        }
    }

    post {
        always {
            sh 'docker-compose down'
        }
    }
}