pipeline {
    agent any

    tools {
 // Define the tool installations
        maven 'Maven'
        jdk 'JDK'  // This should be configured in Jenkins Global Tool Configuration

        // Define Java 17 installation
        jdk name: 'Java17', jdkType: 'jdk', version: '17'  // Customize the name as needed
    
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
            sh "./mvnw clean install"
            env.SONAR_TOKEN = "${SONAR_TOKEN}"
            sh "./mvnw sonar:sonar -Dsonar.projectKey=devops-projectslabs_kyaw -Dsonar.organization=devops-projectslabs -Dsonar.host.url=https://sonarcloud.io -Dsonar.login=${SONAR_TOKEN}"
                }
            }
         }
    }
}
