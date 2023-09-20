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

        stage('Pet clinic build using Maven') {
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

                    // Tag the Docker image
                    tagDockerImage("testingkyaw/${JOB_NAME}")
                }
            }
        }

        stage('Push Docker image to Docker Hub') {
            steps {
                script {
                    // Tag the Docker image
                    tagDockerImage("testingkyaw/${JOB_NAME}")

                    // Push the Docker image
                    pushDockerImage("testingkyaw/${JOB_NAME}")
                }
            }
        }
    }
}

// Function to tag Docker image
def tagDockerImage(repoName) {
    def lowercaseRepoName = repoName.toLowerCase()
    def lowercaseTag = "v1.${BUILD_ID}".toLowerCase()
    def lowercaseLatestTag = "latest"  // Adding a tag for "latest"

    sh """
        cd /var/lib/app
        docker build -t ${lowercaseRepoName}:${lowercaseTag} .
        docker tag ${lowercaseRepoName}:${lowercaseTag} ${lowercaseRepoName}:${lowercaseLatestTag}  # Tag it as "latest"
    """
}

// Function to push Docker image
def pushDockerImage(repoName) {
    def lowercaseRepoName = repoName.toLowerCase()
    def lowercaseTag = "v1.${BUILD_ID}".toLowerCase()
    def lowercaseLatestTag = "latest"

    withCredentials([string(credentialsId: 'Docker_Password', variable: 'Docker_Password')]) {
        // Use triple-single-quotes for multi-line sshCommand
        def sshCommand = """
            sshpass -p '${SSH_PASSWORD}' ssh -o StrictHostKeyChecking=no ${SSH_USERNAME}@${SSH_HOST} <<EOF
            docker login -u testingkyaw -p \${Docker_Password}
            docker push ${lowercaseRepoName}:${lowercaseTag}
            docker push ${lowercaseRepoName}:${lowercaseLatestTag}
            exit
EOF
"""
        sh "${sshCommand}"
    }
}
