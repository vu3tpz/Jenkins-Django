pipeline {
    agent any

    environment {
        DOCKER_COMPOSE_FILE = '/home/ubuntu/Jenkins-Django/docker/deployment/app.yml'
        DJANGO_SETTINGS_MODULE = 'config.settings'
    }

    stages {
        stage('Checkout') {
            steps {
                script {
                    checkout scm
                }
            }
        }

        stage('Cleanup Previous Containers') {
            steps {
                script {
                    // Run the Docker Compose up command using the correct path and environment variables
                    withEnv(["DJANGO_SETTINGS_MODULE=$DJANGO_SETTINGS_MODULE"]) {
                        sh "docker compose -f $DOCKER_COMPOSE_FILE down"
                    }
                }
            }
        }

        stage('Build and Test') {
            steps {
                script {
                    sh "docker compose -f $DOCKER_COMPOSE_FILE build"
                }
            }
        }

        stage('Deploy') {
            steps {
                script {
                    sh "docker compose -f $DOCKER_COMPOSE_FILE up -d"
                }
            }
        }
    }

    post {
        always {
            script {
                // Clean up Docker resources
                sh 'echo "y" | docker system prune -a --volumes'
            }
        }
    }

}