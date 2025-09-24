pipeline {
    agent any

    environment {
        REPO_URL = 'git@github.com:mammarraza/React-NodeAPI-MySQL.git'
        DEPLOY_SERVER = 'root@143.110.189.194'  // Change this
        APP_PATH = '/root/React-NodeAPI-MySQL/frontend'            // Path on remote server where repo is cloned
    }

    stages {
        stage('Checkout Code') {
            steps {
                git branch: 'main', url: "${REPO_URL}"
            }
        }

        stage('Install & Build') {
            steps {
                sh '''
                npm install
                npm run build
                '''
            }
        }

        stage('Deploy to Server') {
            steps {
                sshagent (credentials: ['ammar-server']) {
                    sh """
                    ssh -o StrictHostKeyChecking=no ${DEPLOY_SERVER} '
                        cd ${APP_PATH} &&
                        git pull &&
                        npm install &&
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
