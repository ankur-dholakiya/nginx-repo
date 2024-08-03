pipeline {
    agent any

    stages {
        stage('Checkout SCM') {
            steps {
                // Checkout the current branch from your GitHub repository
                checkout([$class: 'GitSCM', branches: [[name: "${env.GIT_BRANCH}"]], 
                          userRemoteConfigs: [[url: 'https://github.com/ankur-dholakiya/nginx-repo.git']]])
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
                    if (env.GIT_BRANCH == 'origin/dev') {
                        def targetDir = '/home/ubuntu/nginx-repo'
                        def sourceDir = "${env.WORKSPACE}"

                        // Ensure target directory is empty before copying new files
                        sh """
                        # Synchronize the files from source to target directory
                        sudo rsync -av --delete ${sourceDir}/ ${targetDir}/
                        """
                    } else if (env.GIT_BRANCH == 'origin/qa') {
                        def targetDir = '/var/www/html/nginx-repo'
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

        stage('Create Pull Request') {
            when {
                branch 'dev'
            }
            steps {
                script {
                    withCredentials([string(credentialsId: 'github-token', variable: 'GITHUB_TOKEN')]) {
                        sh """
                        curl -X POST -H "Authorization: token ${GITHUB_TOKEN}" -H "Accept: application/vnd.github.v3+json" \
                        https://api.github.com/repos/ankur-dholakiya/nginx-repo/pulls \
                        -d '{
                            "title": "Merge dev into qa",
                            "head": "dev",
                            "base": "qa"
                        }'
                        """
                    }
                }
            }
        }
    }
}
