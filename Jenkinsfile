pipeline {
    agent any

    environment {
        DOCKER_IMAGE = "food-del-app"
        CONTAINER_NAME = "food-del-container"
        PORT = "80"
    }

    stages {

        stage('Clone Repository') {
            steps {
                echo '📥 Cloning repository...'
                checkout scm
            }
        }

        stage('Install Dependencies') {
            steps {
                echo '📦 Installing dependencies...'
                sh 'npm install'
            }
        }

        stage('Build React App') {
            steps {
                echo '🔨 Building React app...'
                sh 'npm run build'
            }
        }

        stage('Build Docker Image') {
            steps {
                echo '🐳 Building Docker image...'
                sh "docker build -t ${DOCKER_IMAGE} ."
            }
        }

        stage('Stop Old Container') {
            steps {
                echo '🛑 Stopping old container if running...'
                sh "docker stop ${CONTAINER_NAME} || true"
                sh "docker rm ${CONTAINER_NAME} || true"
            }
        }

        stage('Run New Container') {
            steps {
                echo '🚀 Running new container...'
                sh "docker run -d --name ${CONTAINER_NAME} -p ${PORT}:80 ${DOCKER_IMAGE}"
            }
        }
    }

    post {
        success {
            echo '✅ Deployment Successful! App is live.'
        }
        failure {
            echo '❌ Build Failed! Check the logs.'
        }
    }
}