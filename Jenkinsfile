pipeline {
    agent any
    environment {
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
                // Add your build steps here if needed
            }
        }
        stage('Deploy') {
            steps {
                script {
                    // Ensure rsync is installed (if you are still using it)
                    sh '''
                    if ! [ -x "$(command -v rsync)" ]; then
                        sudo apt-get update && sudo apt-get install -y rsync
                    fi
                    '''
                    
                    // Deploy the code by copying files to the target directory
                    sh '''
                    mkdir -p ${TARGET_DIR}
                    rsync -av --delete ./ ${TARGET_DIR}/
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
