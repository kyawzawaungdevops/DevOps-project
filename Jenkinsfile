pipeline {
    agent any
    stages {
        stage('Pet clinic build using Maven') {
            steps {
                script {
                    sh "mvn clean install -DskipTests"
                }
            }
        }
        stage('Building a Docker image') {
            steps {
                script {
                    // Build a Docker image
                    sh "docker build -t testingkyaw/pettwo:5.0 ."
                }
            }
        }
        stage('Push image') {
            steps {
                script {
                    // Use your Docker registry URL
                    def registryUrl = "docker push testingkyaw/pettwo"

                    // Push the Docker image
                    withDockerRegistry([credentialsId: "docker-cred", url: registryUrl]) {
                        dockerImage.push()
                    }
                }
            }
        }
    }
}
