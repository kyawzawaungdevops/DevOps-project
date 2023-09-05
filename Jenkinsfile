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
                    sh "docker build -t devops-project/petclinic:1.0 ."
                }
            }
        }

        stage('Pushing the Docker image to Docker Hub') {
            steps {
                script {
                    // Push the Docker image to Docker Hub
                    docker.withRegistry('https://registry.hub.docker.com', 'DockerHub-Secret') {
                        def dockerImage = docker.image('devops-project/petclinic:1.0')
                        dockerImage.push()
                    }
                }
            }
        }
    }
}
