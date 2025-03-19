pipeline {
    agent any

    environment {
        DOCKER_COMPOSE_FILE = 'docker-compose.yml'
    }

    stages {
        stage('Checkout Code') {
            steps {
                echo "Checking out repository..."
                checkout scm
            }
        }

        stage('Pull Docker Images') {
            steps {
                echo "Pulling Docker images..."
                sh 'docker compose pull'
            }
        }

        stage('Deploy ARK Server') {
            steps {
                echo "Deploying ARK server with Docker Compose..."
                sh "docker compose up -d"
            }
        }
    }

    post {
        always {
            echo "Cleaning up dangling Docker images..."
            sh 'docker image prune -f'
        }
    }
}
