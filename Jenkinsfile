pipeline {
    agent any

    stages {

        stage('Pull Code from GitHub') {
            steps {
                echo "Pulling latest code from GitHub..."
                git branch: 'master',
                    url: 'https://github.com/alavalapatikishore/simple-html-app.git'
            }
        }

        stage('Build Docker Image') {
            steps {
                echo "Building Docker image..."
                sh 'docker build -t simple-html-app:latest .'
            }
        }

        stage('Stop & Remove Old Container') {
            steps {
                echo "Stopping old container if exists..."
                sh 'docker rm -f html-app-container || true'
            }
        }

        stage('Run New Container') {
            steps {
                echo "Running new container..."
                sh 'docker run -d --name html-app-container -p 9090:80 simple-html-app:latest'
            }
        }
    }

    post {
        success {
            echo "🎉 Deployment Successful! Visit your app on port 9090"
        }
        failure {
            echo "Pipeline Failed!"
        }
    }
}
