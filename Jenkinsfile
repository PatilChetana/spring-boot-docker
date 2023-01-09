pipeline {
    agent any
    tools{
        maven '3.6.3'
    }

    stages {
        stage('Build Maven') {
            steps {
                sh 'mvn -version'
                checkout scmGit(branches: [[name: '*/master']], extensions: [], userRemoteConfigs: [[url: 'https://github.com/Java-Techie-jt/spring-boot-dockerize.git']])
                sh 'mvn clean install'
            }
        }
        
        stage ('Build Docker Image'){
            steps{
                script{
                    sh 'docker build -t chetana249/spring-boot-docker3-ubuntu .'
                }
            }
        }
        
        stage ('Push to Docker Hub'){
            steps{
                script{
                   withCredentials([string(credentialsId: 'docker-hub-pass', variable: 'dockerhubpass')]) {
                    
                    sh 'docker login -u chetana249 -p ${dockerhubpass}'
                      }
                    sh 'docker push chetana249/spring-boot-docker3-ubuntu'
                }
            }
            
        }
    }
}

