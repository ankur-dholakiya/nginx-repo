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
        
        stage('Setup SSH') {
            steps {
                script {
                    // Ensure the .ssh directory exists
                    sh 'mkdir -p /var/jenkins_home/.ssh'
                    
                    // Add the host key to known_hosts
                    sh 'ssh-keyscan -H 13.234.20.228 >> /var/jenkins_home/.ssh/known_hosts'
                }
            }
        }
        
        stage('Deploy') {
            steps {
                echo 'Deploying...'
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
