pipeline {
    agent any

    tools {
        nodejs 'node'
    }

    environment {
        PORT = '3000'
        IMAGE_NAME = 'nodemain'
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
                sh 'cp src/logo-main.svg src/logo.svg || echo "Using default logo"'
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
                sh "docker run -d -p ${PORT}:3000 --name app-main ${IMAGE_NAME}:v1.0"
            }
        }
    }
}
