pipeline {
    agent any

    tools {
        // Define the tool installations
        maven 'Maven'
        jdk 'JDK'
    }

    environment {
        // Define environment variables
        SONAR_TOKEN = credentials('Sonar_Token')
    }

    stages {
        stage('Install and Configure Tools') {
            steps {
                // Install Maven 3.9.4
                tool name: 'Maven-3.9.4', type: 'maven'

                // Install Java 17 (OpenJDK)
                tool name: 'Java-17', type: 'jdk'

                // Set environment variables
                script {
                    def mavenHome = tool name: 'Maven-3.9.4', type: 'maven'
                    def javaHome = tool name: 'Java-17', type: 'jdk'
                    env.PATH = "${javaHome}/bin:${mavenHome}/bin:${env.PATH}"
                    env.M2_HOME = mavenHome
                }
            }
        }

        stage('Repo Scan using Sonarcloud') {
            steps {
                script {
                    env.SONAR_TOKEN = "${SONAR_TOKEN}"
                    sh "./mvnw sonar:sonar -Dsonar.projectKey=devops-projectslabs_kyaw -Dsonar.organization=devops-projectslabs -Dsonar.host.url=https://sonarcloud.io -Dsonar.login=${SONAR_TOKEN}"
                }
            }
        }
    }
}
