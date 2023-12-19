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
                    sh 'docker image prune -a --force --filter "label=kiran11113/devops-integration"'
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
                sh 'sudo -u devops  kubectl apply -f deploymentservice.yaml'
                sh 'sudo -u devops  kubectl get pods'
               // sh 'kubectl apply -f deploymentservice.yaml'
            }
        }
    }
}


minikube