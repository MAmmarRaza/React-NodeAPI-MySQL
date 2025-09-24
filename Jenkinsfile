pipeline {
    agent any

    environment {
        REPO_URL = 'https://github.com/MAmmarRaza/React-NodeAPI-MySQL.git' // switched to HTTPS
        DEPLOY_SERVER = 'root@143.110.189.194'
        APP_PATH = '/root/React-NodeAPI-MySQL/frontend'
    }

    stages {
        stage('Checkout Code') {
            steps {
                git branch: 'main',
                    credentialsId: 'github-access',  // use your username/password or token credential ID
                    url: "${REPO_URL}"
            }
        }

        stage('Install & Build') {
            steps {
                sh '''
                echo "📦 Installing dependencies..."
                npm install
                echo "🏗️ Building React app..."
                npm run build
                '''
            }
        }

        stage('Deploy to Server') {
            steps {
                sshagent (credentials: ['ammar-server']) {
                    sh """
                    echo "🚀 Deploying to server..."
                    ssh -o StrictHostKeyChecking=no ${DEPLOY_SERVER} '
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
