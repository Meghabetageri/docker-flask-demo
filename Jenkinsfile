pipeline {
    agent any
    environment {
        DOCKERHUB_CREDENTIALS = credentials('dockerhubID')  // Jenkins credentials ID
    }
    stages {
        stage('Build Docker Image') {
            steps {
                script {
                    try {
                        def imageTag = "meghadocker126/flaskapp:${BUILD_NUMBER}"
                        sh "docker build -t ${imageTag} ."
                    } catch (Exception e) {
                        error "Docker build failed: ${e.message}"
                    }
                }
            }
        }
        stage('Login to Docker Hub') {
            steps {
                script {
                    try {
                        sh 'echo $DOCKERHUB_CREDENTIALS_PSW | docker login -u $DOCKERHUB_CREDENTIALS_USR --password-stdin'
                    } catch (Exception e) {
                        error "Docker login failed: ${e.message}"
                    }
                }
            }
        }
        stage('Push Image to Docker Hub') {
            steps {
                script {
                    try {
                        def imageTag = "meghadocker126/flaskapp:${BUILD_NUMBER}"
                        sh "docker push ${imageTag}"
                    } catch (Exception e) {
                        error "Docker push failed: ${e.message}"
                    }
                }
            }
        }
    }
    post {
        always {
            sh 'docker logout'
        }
    }
}
