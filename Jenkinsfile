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
                // Assuming you have SSH access to your server
                sh 'scp -r * ubuntu@your-server-ip:/home/ubuntu/nginx-repo'
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
