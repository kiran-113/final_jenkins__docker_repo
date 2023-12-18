pipeline {
    agent any
    tools{
        maven 'maven_3_5_0'
    }
    stages{
        
        stage('Build Maven'){
            steps{
                checkout([$class: 'GitSCM', branches: [[name: '*/main']], extensions: [], userRemoteConfigs: [[url: 'https://github.com/kiran-113/final_jenkins__docker_repo']]])
                sh 'mvn clean install'
            }
        }
        stage('Build docker image'){
            steps{
                script{
                    sh 'docker build -t kiran11113/devops-integration .'
                }
            }
        }
        stage('Push image to Hub'){
            steps{
                script{
                   withCredentials([string(credentialsId: 'dockerpwd', variable: 'dockerhubpwd')]) {

                   sh 'docker login -u kiran11113 -p ${dockerhubpwd}'
                }
                   sh 'docker push kiran11113/devops-integration'
                }
            }
        }
        stage('Deploy to k8s'){
            steps{
                script{
                   withCredentials([string(credentialsId: 'minikube', variable: 'minikubepwd')]) {

                   sh 'sudo -u devops -p ${minikubepwd} kubectl apply -f deploymentservice.yaml'
                }
                   sh 'kubectl get pods'
                }
              
            }
        }
    }
}