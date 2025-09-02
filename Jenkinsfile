pipeline {
    agent any

    environment {
        DEPLOY_DIR = "/var/www/reactapp"
    }

    stages {

        stage('Checkout') {
            steps {
                // Checkout code from GitHub using Jenkins credentials
                git branch: 'main',
                    url: 'https://github.com/rosebinila37/my_app.git',
                    credentialsId: 'e2f02354-08de-4e31-9f6e-8b6261d52fb4'
            }
        }

        stage('Install Dependencies') {
            steps {
                sh 'npm install'
            }
        }

        stage('Build') {
            steps {
                sh 'npm run build'
            }
        }

        stage('Deploy') {
            steps {
                // Copy build files to web server
                // Use sudo without password for Jenkins or run as www-data user
                sh """
                sudo rm -rf ${DEPLOY_DIR}/*
                sudo cp -r build/* ${DEPLOY_DIR}/
                sudo chown -R www-data:www-data ${DEPLOY_DIR}
                sudo chmod -R 755 ${DEPLOY_DIR}
                """
            }
        }
    }

    post {
        success {
            echo 'Deployment succeeded!'
        }
        failure {
            echo 'Deployment failed!'
        }
    }
}
