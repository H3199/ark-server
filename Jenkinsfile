pipeline {
    agent any

    environment {
        DOCKER_COMPOSE_FILE = 'docker-compose.yml'
	SERVER_PASSWORD = credentials('SERVER_PASSWORD')
        ADMIN_PASSWORD = credentials('ADMIN_PASSWORD')
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
	        script {
                    writeFile file: '.env', text: """
		    ADMIN_PASSWORD=${ARK_ADMIN_PASSWORD}
		    SERVER_PASSWORD=${ARK_SERVER_PASSWORD}
		    """
	        }	
                echo "Deploying ARK server with Docker Compose..."
                sh "docker compose up -d && echo 'cleaning env...' && rm -f .env"
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
