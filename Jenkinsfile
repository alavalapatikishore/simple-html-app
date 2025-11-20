pipeline {
    agent any

    environment {
        IMAGE_NAME = "simple-html"
        CONTAINER_NAME = "html-container"
    }

    stages {

        stage('Pull Latest Code') {
            steps {
                git branch: 'main',
                    url: 'https://github.com/YOUR_USER/simple-html-app.git'
            }
        }

        stage('Build Docker Image') {
            steps {
                sh '''
                docker build -t ${IMAGE_NAME}:latest .
                '''
            }
        }

        stage('Stop Old Container') {
            steps {
                sh '''
                if [ "$(docker ps -q -f name=${CONTAINER_NAME})" ]; then
                    docker stop ${CONTAINER_NAME}
                    docker rm ${CONTAINER_NAME}
                fi
                '''
            }
        }

        stage('Run New Container') {
            steps {
                sh '''
                docker run -d --name ${CONTAINER_NAME} -p 80:80 ${IMAGE_NAME}:latest
                '''
            }
        }
    }

    post {
        success {
            echo "New version deployed successfully!"
        }
    }
}
