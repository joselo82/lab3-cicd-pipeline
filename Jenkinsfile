pipeline {
    agent any

    tools {
        nodejs 'node'
    }

    environment {
        PORT = '3001'
        IMAGE_NAME = 'nodedev'
    }

    stages {
        stage('Checkout') {
            steps {
                checkout scm
            }
        }

        stage('Install Dependencies') {
            steps {
                sh 'npm install'
            }
        }

        stage('Test') {
            steps {
                sh 'npm test'
            }
        }

        stage('Change Logo') {
            steps {
                sh 'cp src/logo-dev.svg src/logo.svg || echo "Using default logo"'
            }
        }

        stage('Build Docker Image') {
            steps {
                sh "docker build -t ${IMAGE_NAME}:v1.0 ."
            }
        }

        stage('Stop Previous Containers') {
            steps {
                sh 'docker stop $(docker ps -q) || true'
                sh 'docker rm $(docker ps -a -q) || true'
            }
        }

        stage('Deploy') {
            steps {
                sh "docker run -d -p ${PORT}:3000 --name app-dev ${IMAGE_NAME}:v1.0"
            }
        }
    }
}
