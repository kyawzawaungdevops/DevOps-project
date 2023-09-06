pipeline {
    agent any

    tools {
        // Define the tool installations
        maven 'Maven-3.9.1'
        jdk 'openjdk-17.0.7'
    }

    environment {
        // Define environment variables
        SONAR_TOKEN = credentials('Sonar_Token')
        Docker_TOKEN = credentials('DockerHub-Secret')
    }

    stages {
        stage('Building a Docker image') {
            steps {
                script {
                    // Build a Docker image
                    sh "docker build -t testingkyaw/petclinic:3.0 ."
                }
            }
        }
        
        stage('Push image') {
            steps {
                script {
                   // Define the Docker image
                   // Removed commented-out code
                   // Configure Docker to use a credential helper
                   sh "echo ${DOCKERHUB_PASSWORD} | docker login -u ${DOCKERHUB_USERNAME} --password-stdin"
                   sh "docker push testingkyaw/petclinic:2.0"
                   sh "docker logout" // Log out after pushing the image
                    }
                }
            }
        }
    }
}
