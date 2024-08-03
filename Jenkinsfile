pipeline {
    agent any // Use any available agent
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
                    // Add your build commands here
                    echo "Building project..."
                }
            }
        }
        stage('Merge Pull Request') {
            steps {
                script {
                    // Merge dev branch into qa branch
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
                    } else {
                        echo "No open PR found to merge."
                    }
                }
            }
        }
        stage('Deploy to Nginx') {
            when {
                branch 'dev'
            }
            steps {
                script {
                    echo "Deploying to Nginx..."
                    // Clone the GitHub repo and deploy to /home/ubuntu/nginx-repo
                    sh '''
                        rm -rf /home/ubuntu/nginx-repo
                        git clone https://github.com/ankur-dholakiya/nginx-repo.git /home/ubuntu/nginx-repo
                    '''
                }
            }
        }
        stage('Deploy to Apache') {
            when {
                branch 'qa'
            }
            steps {
                script {
                    echo "Deploying to Apache..."
                    // Clone the GitHub repo and deploy to /var/www/html/nginx-repo
                    sh '''
                        rm -rf /var/www/html/nginx-repo
                        git clone https://github.com/ankur-dholakiya/nginx-repo.git /var/www/html/nginx-repo
                    '''
                }
            }
        }
    }
    post {
        always {
            cleanWs() // Clean workspace after build
        }
    }
}
