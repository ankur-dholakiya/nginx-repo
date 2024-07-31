pipeline {
    agent any
    environment {
        DEPLOY_DIR = '/home/ubuntu/nginx-repo'
    }
    stages {
        stage('Checkout SCM') {
            steps {
                git url: 'https://github.com/ankur-dholakiya/nginx-repo.git', branch: 'main'
            }
        }
        stage('Build') {
            steps {
                echo 'Building...'
            }
        }
        stage('Test') {
            steps {
                echo 'Testing...'
            }
        }
        stage('Deploy') {
            steps {
                sshagent(credentials: ['your-ssh-credential-id']) {
                    sh """
                    rsync -avz --delete ${WORKSPACE}/ ${DEPLOY_DIR}
                    """
                }
            }
        }
    }
}
