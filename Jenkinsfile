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
sshagent(credentials: ['ubuntu']) {
sh '''
set -eu

EC2_USER="ubuntu"
EC2_HOST="16.171.140.81"
REMOTE_DIR="/home/ubuntu/backend-project-deploy"

echo "Transferring backend code to EC2..."

tar --exclude='.git' \
--exclude='node_modules' \
-czf - . |
ssh -o StrictHostKeyChecking=no \
${EC2_USER}@${EC2_HOST} \
"mkdir -p ${REMOTE_DIR} &&
tar -xzf - -C ${REMOTE_DIR}"

echo "Building and deploying backend..."

ssh -o StrictHostKeyChecking=no \
${EC2_USER}@${EC2_HOST} \
"set -eu
cd ${REMOTE_DIR}

docker stop myapp 2>/dev/null || true
docker rm myapp 2>/dev/null || true

docker build -t backend-image .

docker run -d \
--restart unless-stopped \
-p 5000:5000 \
--name myapp \
backend-image

docker ps --filter name=myapp
echo 'Backend deployment completed successfully.'"
'''
}
}
}
}
}
