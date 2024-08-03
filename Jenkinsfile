pipeline {
    agent any
    environment {
        GITHUB_TOKEN = credentials('github-token') // Replace with your GitHub token credentials ID
    }
    stages {
        stage('Checkout') {
            steps {
                checkout scm
            }
        }
        stage('Build') {
            steps {
                script {
                    // Your build commands here
                }
            }
        }
        stage('Merge Pull Request') {
            steps {
                script {
                    def prNumber = sh(script: '''
                        curl -s -H "Authorization: token ${GITHUB_TOKEN}" -H "Accept: application/vnd.github.v3+json" https://api.github.com/repos/ankur-dholakiya/nginx-repo/pulls | jq .[] | select(.head.ref=="dev") | .number
                    ''', returnStdout: true).trim()
                    
                    if (prNumber) {
                        sh '''
                            curl -X PUT -H "Authorization: token ${GITHUB_TOKEN}" -H "Accept: application/vnd.github.v3+json" https://api.github.com/repos/ankur-dholakiya/nginx-repo/pulls/${prNumber}/merge -d '{
                                "commit_title": "Merging dev into qa",
                                "commit_message": "Automatically merged by Jenkins pipeline",
                                "merge_method": "merge"
                            }'
                        '''
                    }
                }
            }
        }
        stage('Deploy to Nginx') {
            when {
                branch 'dev'
            }
            steps {
                sh '''
                    echo "Deploying to Nginx..."
                    # Add commands to deploy to /home/ubuntu/nginx-repo
                    # Example: rsync or cp command
                '''
            }
        }
        stage('Deploy to Apache') {
            when {
                branch 'qa'
            }
            steps {
                sh '''
                    echo "Deploying to Apache..."
                    # Add commands to deploy to /var/www/html/nginx-repo
                    # Example: rsync or cp command
                '''
            }
        }
    }
    post {
        always {
            cleanWs() // Clean workspace after build
        }
    }
}
