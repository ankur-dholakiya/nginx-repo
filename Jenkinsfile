pipeline {
    agent any

    stages {
        stage('Checkout SCM') {
            steps {
                checkout([$class: 'GitSCM', branches: [[name: '*/dev']],
                          userRemoteConfigs: [[url: 'https://github.com/ankur-dholakiya/nginx-repo.git']]])
            }
        }

        stage('Print Branch Name') {
            steps {
                echo "Branch name: ${env.GIT_BRANCH}"
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

        stage('Deploy to Nginx') {
            when {
                expression {
                    env.GIT_BRANCH ==~ /.*dev$/
                }
            }
            steps {
                echo 'Deploying to Nginx...'
                script {
                    def targetDir = '/home/ubuntu/nginx-repo'
                    def sourceDir = "${env.WORKSPACE}"

                    // Ensure target directory is empty before copying new files
                    sh """
                    sudo rsync -av --delete ${sourceDir}/ ${targetDir}/
                    """
                }
            }
        }

        stage('Create Pull Request') {
            when {
                expression {
                    env.GIT_BRANCH ==~ /.*dev$/
                }
            }
            steps {
                script {
                    withCredentials([string(credentialsId: 'token', variable: 'GITHUB_TOKEN')]) {
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

        stage('Merge Pull Request') {
            when {
                expression {
                    env.GIT_BRANCH ==~ /.*dev$/
                }
            }
            steps {
                script {
                    withCredentials([string(credentialsId: 'token', variable: 'GITHUB_TOKEN')]) {
                        def prNumber = sh(
                            script: "curl -s -H 'Authorization: token ${GITHUB_TOKEN}' -H 'Accept: application/vnd.github.v3+json' https://api.github.com/repos/ankur-dholakiya/nginx-repo/pulls | jq '.[] | select(.head.ref==\"dev\") | .number'",
                            returnStdout: true
                        ).trim()

                        if (prNumber) {
                            sh """
                            curl -X PUT -H "Authorization: token ${GITHUB_TOKEN}" -H "Accept: application/vnd.github.v3+json" \
                            https://api.github.com/repos/ankur-dholakiya/nginx-repo/pulls/${prNumber}/merge \
                            -d '{
                                "commit_title": "Merging dev into qa",
                                "commit_message": "Automatically merged by Jenkins pipeline",
                                "merge_method": "merge"
                            }'
                            """
                        }
                    }
                }
            }
        }

        stage('Deploy to Apache') {
            when {
                expression {
                    env.GIT_BRANCH ==~ /.*qa$/
                }
            }
            steps {
                echo 'Deploying to Apache...'
                script {
                    def targetDir = '/var/www/html/nginx-repo'
                    def sourceDir = "${env.WORKSPACE}"

                    // Ensure target directory is empty before copying new files
                    sh """
                    sudo rsync -av --delete ${sourceDir}/ ${targetDir}/
                    """
                }
            }
        }
    }
}
