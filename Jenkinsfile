pipeline {
    agent any

    environment {
        IMAGE_NAME = "python-devops-app:latest"
    }

    stages {

        stage('Checkout Code') {
            steps {
                git branch: 'main',
                    url: 'https://github.com/giksone/GOJO1.git'
            }
        }

        stage('Use Minikube Docker') {
            steps {
                sh '''
                eval $(minikube docker-env)
                docker info | grep Name
                '''
            }
        }

        stage('Build Docker Image') {
            steps {
                sh '''
                eval $(minikube docker-env)
                docker build -t python-devops-app:latest .
                '''
            }
        }

        stage('Deploy to Kubernetes') {
            steps {
                sh '''
                kubectl apply -f deployment.yaml
                kubectl apply -f service.yaml
                '''
            }
        }

        stage('Check Status') {
            steps {
                sh '''
                kubectl get pods
                kubectl get services
                '''
            }
        }
    }

    post {
        success {
            echo "✅ CI/CD terminé avec succès"
        }
        failure {
            echo "❌ Erreur dans le pipeline"
        }
    }
}
