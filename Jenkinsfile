pipeline {
    agent any

    environment {
        DOCKER_HUB_REPO = 'berahulb11'
        GIT_COMMIT_SHORT = "${env.GIT_COMMIT?.take(7)}"
        BACKEND_IMAGE = "${DOCKER_HUB_REPO}/mern-backend:${GIT_COMMIT_SHORT}"
        FRONTEND_IMAGE = "${DOCKER_HUB_REPO}/mern-frontend:${GIT_COMMIT_SHORT}"
    }

    stages {
        stage('Checkout') {
            steps {
                git branch: 'main', url: 'https://github.com/rahulsaini0160/Container-Orchestration-learnerreport-k8s-helm.git'
            }
        }

        stage('Build Docker Images') {
            steps {
                script {
                    bat 'docker build -t %BACKEND_IMAGE% learnerReportCS_backend'
                    bat 'docker build -t %FRONTEND_IMAGE% learnerReportCS_frontend'
                }
            }
        }

        stage('Push Docker Images to Docker Hub') {
            steps {
                withCredentials([usernamePassword(credentialsId: 'dockerhub-credentials', usernameVariable: 'DOCKER_USER', passwordVariable: 'DOCKER_PASS')]) {
                    script {
                        bat 'echo %DOCKER_PASS% | docker login -u %DOCKER_USER% --password-stdin'
                        bat 'docker push %BACKEND_IMAGE%'
                        bat 'docker push %FRONTEND_IMAGE%'
                    }
                }
            }
        }

        stage('Deploy with Helm') {
            steps {
                bat 'helm upgrade --install learnerreport learnerreport-chart'
            }
        }
    }
}
