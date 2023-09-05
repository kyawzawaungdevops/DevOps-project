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
        stage('Repo Scan using Sonarcloud') {
            steps {
                script {
                    env.SONAR_TOKEN = "${SONAR_TOKEN}"
                    // Execute Sonar analysis
                    sh "mvn verify org.sonarsource.scanner.maven:sonar-maven-plugin:sonar -Dsonar.projectKey=devops-projectslabs_kyaw"
                }
            }
        }

        stage('Pet clinic build using Maven') {
            steps {
                script {
                    // Build the project
                    sh 'mvn clean package -DskipTests=true'
                    sh "mvn compile"
                    sh "mvn package"
                }
            }
        }

        stage('Building a Docker image') {
            steps {
                script {
                    // Build a Docker image
                    sh "docker build -t DevOps-project/petclinic:1.0 ."
                }
            }
        }

        stage('Pushing the Docker image to Docker Hub') {
            steps {
                script {
                    // Log in to Docker Hub and push the image
                    sh "echo $Docker_TOKEN_PSW | docker login -u $Docker_TOKEN_USR --password-stdin"
                    sh "docker push DevOps-project/petclinic:1.0"
                }
            }
        }
    }
}
