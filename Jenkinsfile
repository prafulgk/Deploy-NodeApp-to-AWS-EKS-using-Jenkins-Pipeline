pipeline {
    agent any

    stages {
        stage('Clone') {
            steps {
                git branch: "main", url: "https://github.com/prafulgk/Deploy-NodeApp-to-AWS-EKS-using-Jenkins-Pipeline.git"
            }
        }
    
       stage('Docker Build') {
            steps {
                script {
                    app = docker.build("testimage")
                }
            }
        }
        
        stage('ecr login') {
            steps {
                script {
                    sh "aws ecr get-login-password --region us-east-1 | docker login --username AWS --password-stdin 351182272812.dkr.ecr.us-east-1.amazonaws.com"
                    sh "docker tag testimage:latest 351182272812.dkr.ecr.us-east-1.amazonaws.com/ubuntu:latest"
                    sh "docker push 351182272812.dkr.ecr.us-east-1.amazonaws.com/ubuntu:latest"
                }
            }
        }
        
        stage('eks deploy') {
            steps {
                script {
                    sh "aws eks update-kubeconfig --name FB-Sidecar --region us-east-1"
                    sh "kubectl get ns"
                    sh "kubectl get deploy -A"
                    sh "kubectl apply -f nodejsapp.yaml"
                }
            }
        }
    }
}
