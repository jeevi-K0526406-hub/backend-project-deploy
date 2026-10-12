pipeline {
    agent any
    
    environment{
        EC2_IP = "16.171.140.81"
    }
    stages {
        
        stage('checkout code') {
            steps {
                git branch: 'main',
                credentialsId: 'crm-cred1',
                url: 'https://github.com/jeevi-K0526406-hub/backend-project-deploy.git'
            }
        }
        
        stage('Deploy to EC2') {
            steps {
                sshagent(['backend-ssh']) {
                    sh """
                    ssh -o StrictHostKeyChecking=no ubuntu@${EC2_IP} '
                        set -e
                        mkdir -p /home/ubuntu/apps
                        cd /home/ubuntu/apps

                        if [ ! -d backend-project ]; then
                            git clone git@github.com:greeshmagp19-creator/backend-project.git
                        fi

                        cd backend-project
                        git checkout main
                        git pull origin main
                     
                        docker stop myapp || true
                        docker rm myapp || true
                        docker rmi backend-image || true

                        docker build -t backend-image .
                        docker run -d -p 5000:5000 --name myapp backend-image
                    '
                    """
                }
            }
        }
    }
    
    post {
        success {
            echo "✅ DEPLOYMENT SUCCESSFUL"
        }
        failure {
            echo "❌ DEPLOYMENT FAILED"
        }
    }
}
