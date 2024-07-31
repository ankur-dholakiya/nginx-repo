pipeline {
    agent any

    environment {
        DOCKER_IMAGE = 'nginx:latest'
        DOCKER_CONTAINER_NAME = 'nginx-container'
        DOCKER_PORT = '1234'
    }

    stages {
        stage('Checkout') {
            steps {
                git branch: 'main', url: 'https://github.com/ankur-dholakiya/nginx-repo.git'
            }
        }

        stage('Build Docker Image') {
            steps {
                script {
                    docker.build(DOCKER_IMAGE)
                }
            }
        }

        stage('Deploy Docker Container') {
            steps {
                script {
                    // Stop and remove any existing container
                    sh "docker stop ${DOCKER_CONTAINER_NAME} || true"
                    sh "docker rm ${DOCKER_CONTAINER_NAME} || true"
                    
                    // Run the new container
                    sh "docker run -d --name ${DOCKER_CONTAINER_NAME} -p ${DOCKER_PORT}:80 ${DOCKER_IMAGE}"
                }
            }
        }
    }

    post {
        always {
            cleanWs()
        }
    }
}
