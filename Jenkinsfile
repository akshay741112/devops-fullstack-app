pipeline {
    agent any

    stages {
        stage('Clone Repository') {
            steps {
                withCredentials([
                    usernamePassword(credentialsId: 'jenkins-git-credentials', passwordVariable: 'password', usernameVariable: 'username')
                ]) {
                    git branch: 'main',
                        credentialsId: 'jenkins-git-credentialss',
                        url: 'https://github.com/akshay741112/devops-fullstack-app.git'
                }
            }
        }

        stage('Build Backend') {
            steps {
                dir('backend') {
                    sh 'docker build -t backend-image .'
                }
            }
        }

        stage('Build Frontend') {
            steps {
                dir('frontend') {
                    sh 'docker build -t frontend-image .'
                }
            }
        }

        stage('Deploy to Kubernetes') {
            steps {
                dir('k8s') {
                    script {
                        // Apply backend deployment and service
                        sh 'kubectl apply -f backend-deployment.yaml'
                        sh 'kubectl apply -f backend-service.yaml'

                        // Apply frontend deployment and service
                        sh 'kubectl apply -f frontend-deployment.yaml'
                        sh 'kubectl apply -f frontend-service.yaml'
                    }
                }
            }
        }
    }
}
