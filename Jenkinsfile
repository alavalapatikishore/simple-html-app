pipeline {
    agent any

    environment {
        IMAGE_NAME = "simple-html-image"
        CONTAINER_NAME = "simple-html-container"
    }

    stages {

        stage('Pull Code from GitHub') {
            steps {
                git branch: 'master',
                url: 'https://github.com/alavalapatikishore/simple-html-app.git'
            }
        }

        stage('Build Docker Image') {
            steps {
                sh "docker build -t ${IMAGE_NAME}:latest ."
            }
        }

        stage('Stop & Remove Old Container') {
            steps {
                sh """
                if [ \$(docker ps -aq -f name=${CONTAINER_NAME}) ]; then
                    docker stop ${CONTAINER_NAME} || true
                    docker rm ${CONTAINER_NAME} || true
                fi
                """
            }
        }

        stage('Run New Container') {
            steps {
                sh "docker run -d -p 80:80 --name ${CONTAINER_NAME} ${IMAGE_NAME}:latest"
            }
        }
    }

    post {
        success {
            echo "Deployment Successful!"
        }
        failure {
            echo "Pipeline Failed!"
        }
    }
}
