pipeline {
    agent any
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
        stage('Setup SSH') {
            steps {
                script {
                    sh 'mkdir -p ~/.ssh'
                    sh 'echo "your-ssh-key-content" > ~/.ssh/id_rsa'
                    sh 'chmod 600 ~/.ssh/id_rsa'
                }
            }
        }
        stage('Deploy') {
            steps {
                echo 'Deploying...'
            }
        }
    }
}
