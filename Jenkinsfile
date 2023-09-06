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
        DOCKER_TOKEN = credentials ('docker')
    }
    

    stages {
        stage('Pet clinic build using maven') {
            steps {
                script {
                  sh "mvn clean package -DskipTests=true"
                  sh "mvn compile"
                  sh "mvn package"

        }
      }
    }
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
                    // Configure Docker to use a credential helper
                    withCredentials([usernamePassword(credentialsId: 'docker', usernameVariable: 'DOCKERHUB_USERNAME', passwordVariable: 'DOCKERHUB_PASSWORD')]) {
                        sh "echo ${DOCKERHUB_PASSWORD} | docker login -u ${DOCKERHUB_USERNAME} --password-stdin"
                        sh "docker push testingkyaw/petclinic:3.0"
                        sh "docker logout" // Log out after pushing the image
                    }
                }
            }
        }
    }
}
