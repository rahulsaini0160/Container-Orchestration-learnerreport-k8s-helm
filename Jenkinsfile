pipeline {
    agent any

    environment {
        DOCKER_HUB_REPO = 'berahulb11'
        // We will assign GIT_COMMIT_SHORT inside a script block later
    }

    stages {
        stage('Checkout') {
            steps {
                git branch: 'main', url: 'https://github.com/rahulsaini0160/Container-Orchestration-learnerreport-k8s-helm.git'
            }
        }

        stage('Set Commit Hash') {
            steps {
                script {
                    env.GIT_COMMIT_SHORT = env.GIT_COMMIT?.take(7)
                    env.BACKEND_IMAGE = "${env.DOCKER_HUB_REPO}/mern-backend:${env.GIT_COMMIT_SHORT}"
                    env.FRONTEND_IMAGE = "${env.DOCKER_HUB_REPO}/mern-frontend:${env.GIT_COMMIT_SHORT}"
                }
            }
        }

        stage('Build Docker Images') {
            steps {
                script {
                    // Specify Dockerfile explicitly inside each folder
                    bat """
                        docker build -t ${env.BACKEND_IMAGE} -f learnerReportCS_backend/Dockerfile learnerReportCS_backend
                        docker build -t ${env.FRONTEND_IMAGE} -f learnerReportCS_frontend/Dockerfile learnerReportCS_frontend
                    """
                }
            }
        }

        stage('Push Docker Images to Docker Hub') {
            steps {
                withCredentials([usernamePassword(credentialsId: 'dockerhub-credentials', usernameVariable: 'DOCKER_USER', passwordVariable: 'DOCKER_PASS')]) {
                    script {
                        bat """
                            echo %DOCKER_PASS% | docker login -u %DOCKER_USER% --password-stdin
                            docker push ${env.BACKEND_IMAGE}
                            docker push ${env.FRONTEND_IMAGE}
                        """
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
