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
       //DOCKER_TOKEN = credentials ('docker')
    }
    

    stages {
        stage('Repo Scan using  Sonarcloud'){
         steps {
           script{
            ./mvnw clean install
            env.SONAR_TOKEN = "${SONAR_TOKEN}"
            sh "./mvnw sonar:sonar -Dsonar.projectKey=devops-projectslabs_kyaw -Dsonar.organization=devops-projectslabs -Dsonar.host.url=https://sonarcloud.io -Dsonar.login=${SONAR_TOKEN}"
                }
            }
         }
    }
}
