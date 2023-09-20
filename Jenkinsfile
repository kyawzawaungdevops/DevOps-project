pipeline {
    agent any

    environment {
        // Define environment variables
        SONAR_TOKEN = credentials('Sonar_Token')
        SSH_USERNAME = 'root'
        SSH_PASSWORD = 'Cisco123@cisco'
        SSH_HOST = '172.234.49.203'
    }

    stages {
        stage('Repo Scan using Sonarcloud') {
            steps {
                script {
                    env.SONAR_TOKEN = SONAR_TOKEN
                    sh """
                        ./mvnw sonar:sonar \
                        -Dsonar.projectKey=devops-projectslabs_pet-clinic-app-cicd-pipeline \
                        -Dsonar.organization=devops-projectslabs \
                        -Dsonar.host.url=https://sonarcloud.io \
                        -Dsonar.login=${SONAR_TOKEN}
                    """
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

        stage('Sending Dockerfile to the Ansible server over SSH by Jenkins') {
            steps {
                script {
                    // Use sshpass to provide the SSH password and copy files to the remote server
                    sh "sshpass -p '${SSH_PASSWORD}' scp -r /var/lib/jenkins/workspace/Pet-Clinic-App-CICD-pipeline/target/* ${SSH_USERNAME}@${SSH_HOST}:/var/lib/app"

                    // Convert the tag to lowercase and SSH into the remote server to build the Docker image
                    def lowercaseTag = "${JOB_NAME}:v1.${BUILD_ID}".toLowerCase()
                    def sshCommand = """
                        sshpass -p '${SSH_PASSWORD}' ssh -o StrictHostKeyChecking=no ${SSH_USERNAME}@${SSH_HOST} <<EOF
                        cd /var/lib/app
                        ${lowercaseTag}
                        docker image build -t ${lowercaseTag} .
                        exit
                        EOF
                    """
                    sh "${sshCommand}"
                }
            }
        }
    }
}
