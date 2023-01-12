pipeline {
    agent any
    tools{
        maven '3.6.3'
    }

    stages {
        stage('Build Maven') {
            steps {
                sh 'mvn -version'
                checkout scmGit(branches: [[name: '*/master']], extensions: [], userRemoteConfigs: [[url: 'https://github.com/PatilChetana/spring-boot-docker.git']])
                sh 'mvn clean install'
            }
        }
        
        stage ('Build Docker Image'){
            steps{
                script{
                    sh 'docker build -t chetana249/spring-boot-dockerimg-ubuntu .'
                }
            }
        }
        
        stage ('Push to Docker Hub'){
            steps{
                script{
                   withCredentials([string(credentialsId: 'docker-hub-pass', variable: 'dockerhubpass')]) {
                    
                    sh 'docker login -u chetana249 -p ${dockerhubpass}'
                      }
                    sh 'docker push chetana249/spring-boot-dockerimg-ubuntu'
                }
            }
            
        }
        
        stage ('Deploy Application to K8s'){
            steps{
               git credentialsId: 'docker-hub', url: 'https://github.com/PatilChetana/spring-boot-docker.git' 
               withKubeConfig(caCertificate: '', clusterName: '', contextName: '', credentialsId: 'K8s-ubuntu', namespace: '', restrictKubeConfigAccess: false, serverUrl: '') {
                    sh 'kubectl get nodes'
                    
                    sh 'kubectl apply -f deployments.yaml'
                    sh 'kubectl apply -f services.yaml'
               }
          }
        }
        
        
        
        
    }
}
