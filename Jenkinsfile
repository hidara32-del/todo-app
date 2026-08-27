pipeline {
    agent any

    environment {
        DOCKERHUB_CREDENTIALS = credentials('dockerhub-credentials')
        IMAGE_NAME = 'sarahida/todo-app'
    }

    stages {

        stage('Cloner le dépôt') {
            steps {
                git branch: 'main', url: 'https://github.com/hidara32-del/todo-app.git'
            }
        }

        stage('Lancer les tests unitaires') {
            steps {
                bat 'mvnw.cmd clean test'
            }
        }

        stage('Build de l\'image Docker') {
            steps {
                bat "docker build -t %IMAGE_NAME%:latest ."
            }
        }

        stage('Push vers Docker Hub') {
            steps {
                bat "docker login -u %DOCKERHUB_CREDENTIALS_USR% -p %DOCKERHUB_CREDENTIALS_PSW%"
                bat "docker push %IMAGE_NAME%:latest"
            }
        }

        stage('Déploiement sur Kubernetes') {
            steps {
                bat 'kubectl apply -f kubernetes\\secret.yaml'
                bat 'kubectl apply -f kubernetes\\postgres-pvc.yaml'
                bat 'kubectl apply -f kubernetes\\postgres-deployment.yaml'
                bat 'kubectl apply -f kubernetes\\postgres-service.yaml'
                bat 'kubectl apply -f kubernetes\\deployment.yaml'
                bat 'kubectl apply -f kubernetes\\service.yaml'
            }
        }
    }

    post {
        success {
            echo 'Pipeline exécuté avec succès : clone, tests, build, push et déploiement terminés !'
        }
        failure {
            echo 'Le pipeline a échoué — vérifier les logs ci-dessus.'
        }
    }
}