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
                    def projectRoot = pwd()  // This gets the current directory (subdirectory of the project)

                    // Run Maven with the -Dmaven.multiModuleProjectDirectory property
                    sh """
                        mvn -Dmaven.multiModuleProjectDirectory=$projectRoot clean package -DskipTests=true
                        mvn -Dmaven.multiModuleProjectDirectory=$projectRoot compile
                        mvn -Dmaven.multiModuleProjectDirectory=$projectRoot package
                    """
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

    

