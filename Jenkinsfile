pipeline {
    agent any

    environment {
        DOCKER_USERNAME = "manindersinghdhote" // Docker Hub username
        DOCKER_IMAGE = "tomcat-server"  // Docker image name
        DOCKER_TAG = "latest"  // Docker image tag
    }

    stages {
        stage('Build Docker Image') {
            steps {
                script {
                    // Build the Docker image
                    sh 'docker build -t ${DOCKER_USERNAME}/${DOCKER_IMAGE}:${DOCKER_TAG} .'
                }
            }
        }

        stage('Push Docker Image') {
            steps {
                script {
                    // Use the token from Jenkins credentials
                    withCredentials([string(credentialsId: 'docker-creds', variable: 'DOCKER_HUB_TOKEN')]) {
                        // Authenticate with Docker Hub
                        sh 'echo $DOCKER_HUB_TOKEN | docker login -u $DOCKER_USERNAME --password-stdin'
                        // Push the Docker image
                        sh "docker push ${DOCKER_USERNAME}/${DOCKER_IMAGE}:${DOCKER_TAG}"
                    }
                }
            }
        }

        stage('Clean Up') {
            steps {
                script {
                    // Remove local Docker images to save space
                    sh 'docker rmi ${DOCKER_USERNAME}/${DOCKER_IMAGE}:${DOCKER_TAG}'
                }
            }
        }
    }

    post {
        success {
            echo 'Docker image built and pushed successfully!'
        }
        failure {
            echo 'Docker build or push failed.'
        }
    }
}