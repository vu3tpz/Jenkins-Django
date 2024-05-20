pipeline {
    agent any

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
                    sh 'docker compose -f ./docker/deployment/app.yml down'
                }
            }
        }

        stage('Build and Test') {
            steps {
                script {
                    sh 'docker compose -f ./docker/deployment/app.yml build'
                }
            }
        }

        stage('Deploy') {
            steps {
                script {
                    sh 'docker compose -f ./docker/deployment/app.yml up -d'
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