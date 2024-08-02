pipeline {
    agent any

    stages {
        stage('Checkout SCM') {
            steps {
                git branch: 'dev', url: 'https://github.com/ankur-dholakiya/nginx-repo.git'
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
                script {
                    def targetDir = '/home/ubuntu/nginx-repo'
                    def sourceDir = "${env.WORKSPACE}"

                    sh """
                    sudo cp -r ${sourceDir}/* ${targetDir}/
                    """
                }
            }
        }
    }
}
