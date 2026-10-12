pipeline {
    agent any
    stages {
        stage('Checkout') {
            steps {
                git branch: 'main', credentialsId: 'crm-cred1', url: 'https://github.com/jeevi-K0526406-hub/backend-project-deploy.git'
            }
        }
        stage('Deploy to EC2') {
            steps {
                sshagent(['aws-backend']) {
                    sh """
                    ssh -o StrictHostKeyChecking=no ubuntu@16.171.140.81'
                    set -e
                    mkdir -p /home/ubuntu/backend-project-deploy
                    cd /home/ubuntu/backend-project-deploy
                    rm -rf backend-project-deploy
                    git clone git@github.com:jeevi-K0526406-hub/backend-project-deploy.git
                    cd backend-project-deploy
                    docker stop backend-app || true
                    docker rm backend-app || true
                    docker build -t backend-app .
                    docker run -d --name backend-app -p 5001:5000 backend-app
                    '
                    """
                }
            }
        }
    }
}
