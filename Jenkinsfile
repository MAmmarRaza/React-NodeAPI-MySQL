pipeline {
    agent any

    environment {
        REPO_URL = 'https://github.com/MAmmarRaza/React-NodeAPI-MySQL.git'
        DEPLOY_SERVER = 'root@143.110.189.194'
        APP_PATH = '/root/React-NodeAPI-MySQL/frontend'
    }

    stages {
        stage('Checkout & Build') {
            steps {
                git branch: 'main',
                    credentialsId: 'github-access',
                    url: "${REPO_URL}"
                sh '''
                echo "📦 Installing dependencies..."
                cd frontend
                npm install
                echo "🏗️ Building React app..."
                npm run build
                '''
            }
        }

        stage('Deploy to Server') {
            steps {
                withCredentials([sshUserPrivateKey(credentialsId: 'ammar-server', keyFileVariable: 'SSH_KEY')]) {
                    sh """
                    echo "🚀 Copying build files to server..."
                    scp -i $SSH_KEY -o StrictHostKeyChecking=no -r frontend/build/* ${DEPLOY_SERVER}:${APP_PATH}/build/
                    """
                }
            }
        }
    }

    post {
        success {
            echo "✅ Deployment completed successfully!"
        }
        failure {
            echo "❌ Deployment failed. Check logs."
        }
    }
}
