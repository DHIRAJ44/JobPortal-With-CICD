pipeline {
    agent any

    environment {
        FRONTEND_IMAGE = "mern-frontend:latest"
        BACKEND_IMAGE = "mern-backend:latest"
        NGINX_IMAGE = "mern-nginx:latest"
    }

    stages {
        stage('Checkout') {
            steps {
                git 'https://github.com/DHIRAJ44/JobPortal-With-CICD.git'
        }

        stage('Build Frontend') {
            steps {
                dir('frontend') {
                    script {
                        docker.build("${FRONTEND_IMAGE}")
                    }
                }
            }
        }

        stage('Build Backend') {
            steps {
                dir('backend') {
                    script {
                        docker.build("${BACKEND_IMAGE}")
                    }
                }
            }
        }

        stage('Build Nginx Reverse Proxy') {
            steps {
                dir('nginx') {
                    script {
                        docker.build("${NGINX_IMAGE}")
                    }
                }
            }
        }

        stage('Stop and Remove Old Containers') {
            steps {
                script {
                    sh '''
                    docker stop mern-frontend || true
                    docker stop mern-backend || true
                    docker stop mern-nginx || true

                    docker rm mern-frontend || true
                    docker rm mern-backend || true
                    docker rm mern-nginx || true
                    '''
                }
            }
        }

        stage('Run Containers') {
            steps {
                script {
                    sh '''
                    docker network create mern-network || true

                    docker run -d --name mern-backend --network mern-network mern-backend:latest

                    docker run -d --name mern-frontend --network mern-network mern-frontend:latest

                    docker run -d -p 80:80 --name mern-nginx --network mern-network mern-nginx:latest
                    '''
                }
            }
        }
    }

    post {
        success {
            echo 'MERN App deployed successfully on EC2!'
        }
        failure {
            echo 'Deployment failed.'
        }
    }
}
