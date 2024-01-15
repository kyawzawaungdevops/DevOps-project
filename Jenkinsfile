pipeline {
    agent any
    stages {
        stage('Pet clinic build using maven') {
            steps {
                script {
                  sh "mvn clean install -DskipTests,spring.profiles.active=mysql"
        }
      }
    }
        stage('Building a Docker image') {
            steps {
                script {
                    // Build a Docker image
                    sh "docker build -t testingkyaw/pettwo:4.0 ."
                }
            }
        }
  

    }
}
