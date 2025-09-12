pipeline {
    agent any

    environment {
        IMAGE_NAME = "nginx"
        IMAGE_TAG = "latest"
        KUBE_NAMESPACE = "default"
    }

    stages {
        stage('Checkout') {
            steps {
                checkout scm
            }
        }

        stage('Build Docker Image') {
            steps {
                script {
                    
                    sh 'docker build -t $IMAGE_NAME:$IMAGE_TAG .'
                }
            }
        }

        stage('Deploy to Kubernetes') {
            steps {
                script {
                    
                    sh 'kubectl apply -f nginx-deployment.yaml'
                    sh 'kubectl apply -f nginx-service.yaml'
                }
            }
        }

        stage('Verify Deployment') {
            steps {
                sh 'kubectl get pods -n $KUBE_NAMESPACE'
                sh 'kubectl get svc -n $KUBE_NAMESPACE'
            }
        }
    }
}
