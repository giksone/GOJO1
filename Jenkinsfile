pipeline {
    agent any

    environment {
        DOCKER_IMAGE = "python-devops-app:latest"
    }

    stages {
        stage('Checkout') {
            steps {
                git branch: 'main', url: 'https://github.com/giksone/python-devops-project.git'
            }
        }

        stage('Build Docker Image') {
            steps {
                sh 'eval $(minikube docker-env)'
                sh 'docker build -t $DOCKER_IMAGE .'
            }
        }

        stage('Deploy to Kubernetes') {
            steps {
                sh 'kubectl apply -f deployment.yaml'
                sh 'kubectl apply -f service.yaml'
            }
        }

        stage('Check Pods') {
            steps {
                sh 'kubectl get pods'
                sh 'kubectl get services'
            }
        }
    }

    post {
        success {
            echo "Pipeline terminé ✅"
        }
        failure {
            echo "Pipeline échoué ❌"
        }
    }
}
