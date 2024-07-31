pipeline {
    agent any

    stages {
        stage('Checkout SCM') {
            steps {
                checkout([$class: 'GitSCM',
                          branches: [[name: '*/main']],
                          userRemoteConfigs: [[url: 'https://github.com/ankur-dholakiya/nginx-repo.git']]
                ])
            }
        }
        
        stage('Build') {
            steps {
                echo 'Building...'
                // Add your build steps here
            }
        }
        
        stage('Test') {
            steps {
                echo 'Testing...'
                // Add your test steps here
            }
        }
        
        stage('Deploy') {
            steps {
                echo 'Deploying...'
                // Add the server key to known_hosts (not recommended for production)
                sh 'ssh-keyscan -H 13.234.20.228 >> ~/.ssh/known_hosts'
                sh 'scp -r * ubuntu@13.234.20.228:/home/ubuntu/nginx-repo'
            }
        }
        
        stage('Post Actions') {
            steps {
                echo 'Cleaning up...'
                // Add your cleanup steps here
            }
        }
    }
}
