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
                        ./mvnw sonar:sonar \\
                        -Dsonar.projectKey=devops-projectslabs_pet-clinic-app-cicd-pipeline \\
                        -Dsonar.organization=devops-projectslabs \\
                        -Dsonar.host.url=https://sonarcloud.io \\
                        -Dsonar.login=\${SONAR_TOKEN}
                    """
                }
            }
        }
        stage('Pet clinic build using Maven') {
            steps {
                script {
                    sh "./mvnw clean install"
                }
            }
        }
   stage('Deploying Kubernetes Manifests on Ansible Server over SSH by Jenkins') {
            steps {
                script {
                    // Use sshpass to provide the SSH password and copy files to the remote server
                    sh "sshpass -p '${SSH_PASSWORD}' scp -r /var/lib/jenkins/workspace/Pet-Clinic-App-CICD-pipeline/Kubernetes/* ${SSH_USERNAME}@${SSH_HOST}:/var/lib/app"
                    def sshCommand = """
                        sshpass -p '${SSH_PASSWORD}' ssh -o StrictHostKeyChecking=no ${SSH_USERNAME}@${SSH_HOST} <<EOF
                        cd /var/lib/app
                        export KUBECONFIG=/root/kubeconfig.yaml
                        kubectl apply -f deployment.yaml
                        exit
EOF
"""
                    sh "${sshCommand}"
                }
            }
        }
    }
}
