pipeline {
    agent any


    environment {
        // Define environment variables
        SONAR_TOKEN = credentials('Sonar_Token')
    }

    stages {


        stage('Repo Scan using Sonarcloud') {
            steps {
                script {
                    env.SONAR_TOKEN = "${SONAR_TOKEN}"
                    //sh "./mvnw clean install"
                    sh "./mvnw sonar:sonar -Dsonar.projectKey=devops-projectslabs_pet-clinic-app-cicd-pipeline -Dsonar.organization=devops-projectslabs -Dsonar.host.url=https://sonarcloud.io -Dsonar.login=${SONAR_TOKEN}"
                }
            }
        }

            stage('Pet clinic build using maven') {
            steps {
                script {
                 sh "./mvnw clean install"
        }
      }
    }

        stage ('sending dockerfile to the ansible server over ssh by jenkins'){
        
        sshagent(['ansible']) {
            sh 'ssh -o StrictHostKeyChecking=no ubuntu@172.234.49.203'
            sh 'scp /var/lib/jenkins/workspace/Pet-Clinic-App-CICD-pipeline/* ubuntu@43.204.144.251:/home/ubuntu/'
        }
        
    }
    }
}
