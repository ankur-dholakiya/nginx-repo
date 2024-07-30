pipeline {
    agent any
    stages {
        stage('Checkout') {
            steps {
                // Check out the code from Git
                checkout scm
            }
        }
        stage('Build') {
            steps {
                // Perform any build steps if necessary
                echo "Building code..."
            }
        }
        stage('Deploy') {
            steps {
                script {
                    // Use SSH or another method to deploy the code
                    sh 'rsync -av --delete ./ /home/ubuntu/my-nginx-repo/'
                }
            }
        }
    }
    post {
        always {
            // Clean up workspace after build
            cleanWs()
        }
    }
}
