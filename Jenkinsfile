pipeline {
    agent any
    environment {
        SERVER_USER = 'ubuntu' // Change this if your username is different
        SERVER_IP = 'http://65.1.131.103/' // Replace with your server's IP
        TARGET_DIR = '/home/ubuntu/nginx-repo'
        GIT_REPO = 'https://github.com/ankur-dholakiya/nginx-repo.git'
    }
    stages {
        stage('Checkout') {
            steps {
                git branch: 'main', url: "${env.GIT_REPO}"
            }
        }
        stage('Build') {
            steps {
                echo 'Building code...'
                // Add your build steps here
            }
        }
        stage('Deploy') {
            steps {
                script {
                    // Ensure the target directory exists and deploy the code using rsync
                    sh '''
                    ssh -o StrictHostKeyChecking=no ${SERVER_USER}@${SERVER_IP} "mkdir -p ${TARGET_DIR}"
                    rsync -av --delete -e "ssh -o StrictHostKeyChecking=no" ./ ${SERVER_USER}@${SERVER_IP}:${TARGET_DIR}/
                    '''
                }
            }
        }
    }
    post {
        always {
            cleanWs()
        }
    }
}
