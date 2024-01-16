pipeline {
    agent any

    stages {
        stage('Pet clinic build using Maven') {
            steps {
                script {
                    sh 'mvn clean install -DskipTests'
                }
            }
        }

        stage('Building a Docker image') {
            steps {
                script {
                    // Build a Docker image
                    sh "docker build -t testingkyaw/pettwo:${BUILD_NUMBER} ."
                }
            }
        }

        stage('Build and Push Docker Image') {

            steps {
                script {
                    // Push the Docker image to the registry
                    docker.withRegistry('https://index.docker.io/v1/', 'docker-cred') {
                        sh "docker push testingkyaw/pettwo:${BUILD_NUMBER}"
                    }
                }
            }
        }
    }
}
