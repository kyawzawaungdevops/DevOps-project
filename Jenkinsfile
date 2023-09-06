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
                    sh "docker build -t testingkyaw/petclinic:2.0 ."
                }
            }
        }
        
        stage('Push image') {
            steps {
                script {
                    // Define the Docker image
                    def dockerImage = docker.build('testingkyaw/petclinic:2.0')
                    // Push the Docker image to Docker Hub
                    withDockerRegistry([credentialsId: "docker", url: "https://index.docker.io/"]) {
                    dockerImage.push()
                    }
                }
            }
        }
    }
}
