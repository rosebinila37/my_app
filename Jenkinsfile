pipeline {
    agent any

    environment {
        APP_DIR = "/var/lib/jenkins/workspace/reactapp"  // Jenkins workspace
        DEPLOY_DIR = "/var/www/reactapp"                 // NGINX root folder
    }

    stages {
        stage('Checkout') {
            steps {
                // Pull latest code from Git
                git branch: 'main', url: 'https://github.com/rosebinila37/my_app.git'
            }
        }

        stage('Install Dependencies') {
            steps {
                // Make sure Node.js is available
                sh 'npm install'
            }
        }

        stage('Build') {
            steps {
                // Build the React app
                sh 'npm run build'
            }
        }

        stage('Deploy') {
            steps {
                // Copy build files to NGINX directory and reload
                sh "sudo cp -r build/* ${DEPLOY_DIR}/"
                sh "sudo systemctl reload nginx"
            }
        }
    }

    post {
        success {
            echo "Deployment Successful!"
        }
        failure {
            echo "Deployment Failed!"
        }
    }
}
