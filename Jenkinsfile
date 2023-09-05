pipeline {
    agent any
        tools {
        maven 'Maven-3.9.1'
        jdk 'openjdk-17.0.7'
    }
    environment {
        SONAR_TOKEN = credentials('Sonar_Token')
        Docker_TOKEN = credentials('DockerHub-Secret')
    }
    stages {

      stage('Pet clinic build using maven') {
        steps {
            script {
            
            sh 'mvn clean package -DskipTests=true -Dmaven.multiModuleProjectDirectory=/var/lib/jenkins/workspace/Docker\Image\Build\pipeline'
            sh "mvn compile"
            sh "mvn package"

        }
    }
    }
    stage('building a docker image') {
        steps{
            script {
                sh "docker build -t DevOps-project/petclinic:1.0 ."
            }
        }
    }

    stage ('pushing the docker image to dockerhub') {
        steps{
            script{
                sh "echo $Docker_TOKEN_PSW | docker login -u $Docker_TOKEN_USR --password-stdin"
                sh "docker push PetClinic_Part2/petclinic:1.0"
            }
        }
    }

        
    }

    }
