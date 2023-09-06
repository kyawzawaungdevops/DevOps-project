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
        
        stage('Push image') {
            steps {
                script {
                    // Push the Docker image to Docker Hub
                    withDockerRegistry([credentialsId: "DockerHub-Secret", url: "https://hub.docker.com/repository/docker/testingkyaw/devops-project/"]) {
                        dockerImage.push()
                    }
                }
            }
        }
    }
}
