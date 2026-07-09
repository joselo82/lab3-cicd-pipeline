pipeline {
    agent any

    tools {
        nodejs 'node'
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

        stage('Build Docker Image') {
            steps {
                script {
                    if (env.BRANCH_NAME == 'main') {
                        sh 'docker build -t nodemain:v1.0 .'
                    } else if (env.BRANCH_NAME == 'dev') {
                        sh 'docker build -t nodedev:v1.0 .'
                    }
                }
            }
        }

        stage('Stop Previous Containers') {
            steps {
                sh 'docker stop $(docker ps -q) || true'
                sh 'docker rm $(docker ps -a -q) || true'
            }
        }

        stage('Run Application') {
            steps {
                script {
                    if (env.BRANCH_NAME == 'main') {
                        sh 'docker run -d --expose 3000 -p 3000:3000 nodemain:v1.0'
                    } else if (env.BRANCH_NAME == 'dev') {
                        sh 'docker run -d --expose 3001 -p 3001:3000 nodedev:v1.0'
                    }
                }
            }
        }
    }
}
