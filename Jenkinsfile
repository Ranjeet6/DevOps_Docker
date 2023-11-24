pipeline {
    agent any
    
    environment {
        DOCKER_HUB_CREDENTIALS = 'bf153ce8-4048-4ca4-b781-830c87ec605b'
        DOCKER_IMAGE_NAME = 'ranjeet6/your-java-app'
        DOCKER_IMAGE_TAG = "${DOCKER_IMAGE_NAME}:${env.BUILD_NUMBER}"
    }

    stages {
        stage('Checkout') {
            steps {
                script {
                    git 'https://github.com/Ranjeet6/DevOps_Docker.git'
                }
            }
        }
        
        stage('Build') {
            steps {
                script {
                    bat(script: 'javac Main.java', returnStatus: true)
                    archiveArtifacts artifacts: '**/*.class', fingerprint: true
                }
            }
        }
        
        stage('Test') {
            steps {
                script {
                    bat(script: 'java Main', returnStatus: true)
                }
            }
        }

        stage('Build and Push Docker Image') {
            steps {
                script {
                    // Build Docker image
                    def dockerImage = docker.build("${DOCKER_IMAGE_TAG}")
                    
                    // Authenticate with Docker Hub
                    docker.withRegistry('https://registry.hub.docker.com', "${DOCKER_HUB_CREDENTIALS}") {
                        // Push Docker image to Docker Hub
                        dockerImage.push()
                    }
                }
            }
        }
    }
    
    post {
        success {
            echo 'Build and Docker image push successful!'
        }
        
        failure {
            echo 'Build or Docker image push failed!'
        }
    }
}
