pipeline {
    agent any

    environment {
        // Define environment variables here
        REGISTRY_URL = 'https://index.docker.io/v1/'
        DOCKER_IMAGE_NAME = "testingkyaw/pettwo"
        DOCKER_IMAGE_TAG = "${DOCKER_IMAGE_NAME}:${BUILD_NUMBER}"
        REGISTRY_CREDENTIALS = credentials('docker-cred')
    }

    stages {
        stage('Pet clinic build using Maven') {
            steps {
                script {
                    // Execute Maven build
                    sh "mvn clean install -DskipTests"
                }
            }
        }

        stage('Building a Docker image') {
            steps {
                script {
                    // Build a Docker image
                    sh "docker build -t ${DOCKER_IMAGE_TAG} ."
                }
            }
        }

        stage('Build and Push Docker Image') {
            steps {
                script {
                    // Push the Docker image to the registry
                    docker.withRegistry(REGISTRY_URL, REGISTRY_CREDENTIALS) {
                        sh "docker push ${DOCKER_IMAGE_TAG}"
                    }
                }
            }
        }
    }
}
