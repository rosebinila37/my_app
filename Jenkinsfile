pipeline {
    agent any

    stages {
        stage('Checkout') {
            steps {
                git branch: 'main',
                    url: 'https://github.com/rosebinila37/my_app.git'
            }
        }

        stage('Docker Login & Build') {
            steps {
                withCredentials([
                    usernamePassword(
                        credentialsId: 'dockerhub-credentials',
                        usernameVariable: 'DOCKER_USER',
                        passwordVariable: 'DOCKER_PASS'
                    )
                ]) {
                    sh '''
                        echo "Logging into DockerHub..."
                        echo $DOCKER_PASS | docker login -u $DOCKER_USER --password-stdin
                        echo "Building Docker image..."
                        docker build -t $DOCKER_USER/myapp:${BUILD_NUMBER} -t $DOCKER_USER/myapp:latest .
                    '''
                }
            }
        }

        stage('Push Image') {
            steps {
                sh '''
                    docker push $DOCKER_USER/myapp:${BUILD_NUMBER}
                    docker push $DOCKER_USER/myapp:latest
                '''
            }
        }

        stage('Deploy to Minikube') {
            steps {
                withCredentials([file(credentialsId: 'minikube-kubeconfig', variable: 'KUBECONFIG_FILE')]) {
                    sh '''
                        export KUBECONFIG=$KUBECONFIG_FILE
                        kubectl config use-context minikube
                        kubectl set image deployment/myapp-deployment \
                          myapp-container=$DOCKER_USER/myapp:${BUILD_NUMBER}
                    '''
                }
            }
        }
    }
}
