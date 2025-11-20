pipeline {
    agent any

    environment {
        DOCKER_IMAGE = "simple-html-app"
        GITHUB_URL = "https://github.com/alavalapatikishore/simple-html-app.git"
        BRANCH = "master"
        CONTAINER_NAME = "html-app-container"
    }

    stages {

        stage('Pull Code from GitHub') {
            steps {
                echo "Pulling latest code from GitHub..."
                git branch: "${BRANCH}", url: "${GITHUB_URL}"
            }
        }

        stage('Build Docker Image') {
            steps {
                echo "Building Docker image..."
                sh """
                    docker build -t ${DOCKER_IMAGE}:latest .
                """
            }
        }

        stage('Stop & Remove Old Container') {
            steps {
                echo "Stopping old container if exists..."
                sh """
                    docker rm -f ${CONTAINER_NAME} || true
                """
            }
        }

        stage('Run New Container') {
            steps {
                echo "Running new container..."
                sh """
                    docker run -d --name ${CONTAINER_NAME} -p 8080:80 ${DOCKER_IMAGE}:latest
                """
            }
        }
    }

    post {
        success {
            echo "Pipeline Success! App running on port 8080"
        }
        failure {
            echo "Pipeline Failed!"
        }
    }
}
