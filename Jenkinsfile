pipeline {
    agent any

    environment {
        DOCKER_HUB_REPO = 'berahulb11'
        BACKEND_IMAGE = "${DOCKER_HUB_REPO}/mern-backend"
        FRONTEND_IMAGE = "${DOCKER_HUB_REPO}/mern-frontend"
    }

    stages {
        stage('Checkout') {
            steps {
                git 'https://github.com/rahulsaini0160/Container-Orchestration-learnerreport-k8s-helm.git'
            }
        }

        stage('Build Docker Images') {
            steps {
                script {
                    sh 'docker build -t $BACKEND_IMAGE ./learnerReportCS_backend'
                    sh 'docker build -t $FRONTEND_IMAGE ./learnerReportCS_frontend'
                }
            }
        }

        stage('Push Docker Images to Docker Hub') {
            steps {
                withCredentials([usernamePassword(credentialsId: 'dockerhub-credentials', usernameVariable: 'DOCKER_USER', passwordVariable: 'DOCKER_PASS')]) {
                    script {
                        sh 'echo $DOCKER_PASS | docker login -u $DOCKER_USER --password-stdin'
                        sh 'docker push $BACKEND_IMAGE'
                        sh 'docker push $FRONTEND_IMAGE'
                    }
                }
            }
        }

        stage('Deploy with Helm') {
            steps {
                sh 'helm upgrade --install learnerreport ./learnerreport-chart'
            }
        }
    }
}
