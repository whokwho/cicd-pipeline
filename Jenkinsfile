pipeline {
    agent any
    tools { nodejs 'Node-7.8.0' }

    environment {
        APP_PORT = "${env.BRANCH_NAME == 'main' ? '3000' : '3001'}"
        IMAGE_NAME = "${env.BRANCH_NAME == 'main' ? 'nodemain' : 'nodedev'}"
    }

    stages {
        stage('Checkout') {
            steps { checkout scm }
        }
        stage('Build') {
            steps { sh 'npm install' }
        }
        stage('Test') {
            steps { sh 'npm test || true' }
        }
        stage('Build Docker Image') {
            steps {
                sh "docker build -t ${IMAGE_NAME}:v1.0 ."
            }
        }
        stage('Deploy') {
            steps {
                sh """
                  docker rm -f ${IMAGE_NAME}_container || true
                  docker run -d --name ${IMAGE_NAME}_container -p ${APP_PORT}:${APP_PORT} -e PORT=${APP_PORT} ${IMAGE_NAME}:v1.0
                """
            }
        }
    }
}
