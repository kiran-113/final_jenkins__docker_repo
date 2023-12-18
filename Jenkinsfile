pipeline {
    agent any
    tools{
        maven 'maven_3_5_0'
    }
    stages{
        stage('Environment') {
            steps {
                env {
                BUILD_NUMBER = new Random().nextInt(100000)
                }
            }
        }
        stage('Build Maven'){
            steps{
                checkout([$class: 'GitSCM', branches: [[name: '*/main']], extensions: [], userRemoteConfigs: [[url: 'https://github.com/kiran-113/final_jenkins__docker_repo']]])
                sh 'mvn clean install'
            }
        }
        stage('Build docker image'){
            steps{
                script{
                    sh 'docker build -t kiran11113/devops-integration:$BUILD_NUMBER .'
                }
            }
        }
        stage('Push image to Hub'){
            steps{
                script{
                   withCredentials([string(credentialsId: 'dockerpwd', variable: 'dockerhubpwd')]) {
                   sh 'docker login -u kiran11113 -p ${dockerhubpwd}'

}
                   sh 'docker push kiran11113/devops-integration:$BUILD_NUMBER'
                }
            }
        }
        stage('Deploy to k8s'){
            steps{
                sh 'sudo -u devops minikube deploy kubectl apply -f deploymentservice.yaml'
                //sh 'kubectl apply -f deploymentservice.yaml'
            }
        }
    }
}