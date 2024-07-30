pipeline {
    agent any
    environment {
        SERVER_USER = 'ubuntu'
        SERVER_IP = '65.1.131.103'
        TARGET_DIR = '/home/ubuntu/nginx-repo'
        GIT_REPO = 'https://github.com/ankur-dholakiya/nginx-repo.git'
        SSH_CREDENTIALS_ID = 'deploy-ssh-key' // Replace this with the actual credentials ID
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
                // Add your build steps here if needed
            }
        }
        stage('Deploy') {
            steps {
                script {
                    // Ensure rsync is installed
                    sh '''
                    if ! [ -x "$(command -v rsync)" ]; then
                        sudo apt-get update && sudo apt-get install -y rsync
                    fi
                    '''
                    
                    // Deploy the code
                    withCredentials([sshUserPrivateKey(credentialsId: "${SSH_CREDENTIALS_ID}", keyFileVariable: 'SSH_KEY')]) {
                        sh '''
                        ssh-agent bash -c 'ssh-add <(echo "${SSH_KEY}"); rsync -av --delete -e "ssh -o StrictHostKeyChecking=no" ./ ${SERVER_USER}@${SERVER_IP}:${TARGET_DIR}/'
                        '''
                    }
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
