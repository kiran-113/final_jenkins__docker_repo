# devops-automation

#https://github.com/kiran-113/final_jenkins__docker_repo.git

manage jenkins --> tools --> maven version 3.5.0

toget kubeconfig file location : [[! -z "$KUBECONFIG"]] && echo "$KUBECONFIG" || echo "$HOME/.kube/config"

stage('Deploy to k8s'){
steps{
script{
kubernetesDeploy (configs: 'deploymentservice.yaml',kubeconfigId: 'k8sconfigpwd')
}
}
}
