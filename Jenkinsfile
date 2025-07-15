pipeline {
    agent any
    environment {
        DOCKER_IMAGE = 'manjukolkar007/test-image:latest'
    }
    stages {
        stage('Clone Repository') {
            steps {
                git 'https://github.com/manjukolkar/scroll-web.git'
            }
        }
        stage('Build Docker Image') {
            steps {
                sh 'docker build -t $DOCKER_IMAGE .'
            }
        }
        stage('Login to Docker Hub') {
            steps {
                script {
                    withCredentials([usernamePassword(credentialsId: 'docker-hub-credentials', usernameVariable: 'DOCKER_USERNAME', passwordVariable: 'DOCKER_PASSWORD')]) {
                        sh "echo $DOCKER_PASSWORD | docker login -u $DOCKER_USERNAME --password-stdin"
                    }
                }
            }
        }
        stage('Push Docker Image') {
            steps {
                sh 'docker push $DOCKER_IMAGE'
            }
        }
        stage('Kubernetes Deployment') {
            steps {
                sh 'microk8s.kubectl apply -f k8s/deployment.yaml'
            }
        }
        stage('Kubernetes Ingress Setup') {
            steps {
                sh 'microk8s.kubectl apply -f k8s/ingress.yaml'
            }
        }
    }
}

