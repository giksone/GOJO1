pipeline {
    agent any

    environment {
        // Nom de l'image Docker avec le hash du commit pour forcer Kubernetes à la prendre
        IMAGE_NAME = "python-devops-app:${GIT_COMMIT}"
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
                # Pointez Docker sur le daemon Minikube
                eval $(minikube docker-env)
                docker info | grep Name
                '''
            }
        }

        stage('Build Docker Image') {
            steps {
                sh '''
                eval $(minikube docker-env)
                docker build -t $IMAGE_NAME .
                '''
            }
        }

        stage('Deploy to Kubernetes') {
            steps {
                sh '''
                # Appliquer les fichiers Kubernetes
                kubectl apply -f deployment.yaml
                kubectl apply -f service.yaml

                # Mettre à jour le déploiement avec la nouvelle image
                kubectl set image deployment/python-app python-app=$IMAGE_NAME

                # Forcer le redéploiement pour tirer la nouvelle image
                kubectl rollout restart deployment/python-app

                # Vérifier l'état des pods
                kubectl get pods
                kubectl get services
                '''
            }
        }
    }

    post {
        success {
            echo "✅ CI/CD terminé avec succès. Tous les pods sont à jour."
        }
        failure {
            echo "❌ Erreur dans le pipeline. Vérifier les logs."
        }
    }
}
