pipeline {
    agent any

    environment {
        REPO_URL = 'https://github.com/MAmmarRaza/React-NodeAPI-MySQL.git'
        DEPLOY_SERVER = 'root@143.110.189.194'
        APP_PATH = '/root/React-NodeAPI-MySQL/frontend'
    }

    stages {
        stage('Deploy to Server') {
            steps {
                withCredentials([sshUserPrivateKey(credentialsId: 'ammar-server', keyFileVariable: 'SSH_KEY')]) {
                    sh """
                    echo "🚀 Deploying to server..."
                    ssh -i $SSH_KEY -o StrictHostKeyChecking=no ${DEPLOY_SERVER} '
                        cd ${APP_PATH} &&
                        echo "🔄 Pulling latest code..." &&
                        git pull &&
                        echo "📦 Installing dependencies..." &&
                        npm install &&
                        echo "🏗️ Building React app..." &&
                        npm run build
                    '
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
