pipeline {
    agent any
    
    tools {
        maven 'Maven-3.9.1'
        jdk 'openjdk-17.0.7'
    }
    
    environment {
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
                    docker.withRegistry('https://hub.docker.com/repository/docker/testingkyaw/devops-project/', 'DockerHub-Secret') {
                    dockerImage.push()
                }
            }
        }
    }
}
