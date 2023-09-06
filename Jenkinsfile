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
        stage('Repo Scan using  Sonarcloud'){
         steps {
           script{
            env.SONAR_TOKEN = "${SONAR_TOKEN}"
            sh "./mvnw verify org.sonarsource.scanner.maven:sonar-maven-plugin:sonar -Dsonar.projectKey=devops-projectslabs_kyaw -DskipTests=true"
      }
     }
        }
        stage('Pet clinic build using maven') {
            steps {
                script {
                  sh "./mvnw package -DskipTests=true"
        }
      }
    }
        stage('Building a Docker image') {
            steps {
                script {
                    // Build a Docker image
                    sh "docker build -t testingkyaw/petclinic:4.0 ."
                }
            }
        }


        
        stage('Push image') {
            steps {
                script {
                    // Configure Docker to use a credential helper
                    withCredentials([usernamePassword(credentialsId: 'docker', usernameVariable: 'DOCKERHUB_USERNAME', passwordVariable: 'DOCKERHUB_PASSWORD')]) {
                        sh "echo ${DOCKERHUB_PASSWORD} | docker login -u ${DOCKERHUB_USERNAME} --password-stdin"
                        sh "docker push testingkyaw/petclinic:4.0"
                        sh "docker logout" // Log out after pushing the image
                    }
                }
            }
        }
    }
}
