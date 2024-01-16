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
                    sh "docker build -t testingkyaw/pettwo:${BUILD_NUMBER} ."
                }
            }
        }

    }
}
