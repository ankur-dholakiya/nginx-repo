pipeline {
    agent any

    stages {
        stage('Checkout SCM') {
            steps {
                // Checkout the 'dev' branch from your GitHub repository
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

                    // Ensure target directory is empty before copying new files
                    sh """
                    # Synchronize the files from source to target directory
                    sudo rsync -av --delete ${sourceDir}/ ${targetDir}/
                    """
                }
            }
        }
    }
}
