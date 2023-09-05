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
     stage('Repo Scan using  Sonarcloud'){
         steps {
           script{
          env.SONAR_TOKEN = "${SONAR_TOKEN}"
          //sh "mvn verify sonar:sonar -Dsonar.projectKey=shruti9742327_petclinic3"
          sh "mvn verify org.sonarsource.scanner.maven:sonar-maven-plugin:sonar -Dsonar.projectKey=devops-projectslabs_kyaw"
         }
      }


      stage('Pet clinic build using maven') {
        steps {
            script {
            
            sh 'mvn clean package -DskipTests=true -Dmaven.multiModuleProjectDirectory=/var/lib/jenkins/workspace/Docker Image Build pipeline'
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
