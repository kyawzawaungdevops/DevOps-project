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
                    def customImage = docker.build('devops-project/petclinic:1.0')
                    // Tag and push the image
                    customImage.push()
                }
            }
        }

    }
}
