pipeline {
    agent any 

    stages {
        stage('Checkout') {
            steps {
                // Checkout the code from GitHub
                checkout([$class: 'GitSCM',
                          userRemoteConfigs: [[url: 'https://github.com/ankur-dholakiya/nginx-repo.git']],
                          branches: [[name: '*/main']]
                ])
            }
        }

        stage('Build') {
            steps {
                echo 'Building...'
                // Add build steps here (e.g., make, mvn, npm install, etc.)
            }
        }

        stage('Test') {
            steps {
                echo 'Testing...'
                // Add test steps here (e.g., running unit tests)
            }
        }

        stage('Deploy') {
            steps {
                echo 'Deploying...'
                // Add deployment steps here (e.g., deployment scripts)
            }
        }
    }

    post {
        always {
            echo 'Cleaning up...'
            // Clean up steps if necessary (e.g., deleting temporary files)
        }
    }
}
