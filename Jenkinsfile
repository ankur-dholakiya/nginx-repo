pipeline {
    agent any
    environment {
        GITHUB_TOKEN = credentials('github-token') // Replace with your GitHub token credentials ID
    }
    stages {
        stage('Checkout') {
            steps {
                script {
                    checkout scm
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
                    sh '''
                        rm -rf /home/ubuntu/nginx-repo || true
                        git clone https://github.com/ankur-dholakiya/nginx-repo.git /home/ubuntu/nginx-repo
                    '''
                }
            }
        }
        stage('Create Pull Request') {
            when {
                branch 'dev'
            }
            steps {
                script {
                    echo "Creating pull request from dev to qa..."
                    def prNumber = sh(script: '''
                        curl -s -X POST -H "Authorization: token ${GITHUB_TOKEN}" -H "Accept: application/vnd.github.v3+json" https://api.github.com/repos/ankur-dholakiya/nginx-repo/pulls -d '{
                            "title": "Merge dev into qa",
                            "head": "dev",
                            "base": "qa",
                            "body": "Automatically created by Jenkins pipeline"
                        }' | jq -r '.number'
                    ''', returnStdout: true).trim()
                    
                    if (prNumber) {
                        echo "Pull request created: #${prNumber}"
                    } else {
                        error "Failed to create pull request."
                    }
                }
            }
        }
        stage('Merge Pull Request') {
            when {
                branch 'dev'
            }
            steps {
                script {
                    echo "Merging pull request from dev to qa..."
                    def prNumber = sh(script: '''
                        curl -s -H "Authorization: token ${GITHUB_TOKEN}" -H "Accept: application/vnd.github.v3+json" https://api.github.com/repos/ankur-dholakiya/nginx-repo/pulls | jq -r '.[] | select(.head.ref=="dev" and .base.ref=="qa") | .number'
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
        stage('Deploy to Apache') {
            when {
                branch 'qa'
            }
            steps {
                script {
                    echo "Deploying to Apache..."
                    sh '''
                        rm -rf /var/www/html/nginx-repo || true
                        git clone https://github.com/ankur-dholakiya/nginx-repo.git /var/www/html/nginx-repo
                    '''
                }
            }
        }
    }
    post {
        always {
            script {
                echo "Post build actions..."
                // Skipped clean up
            }
        }
    }
}
