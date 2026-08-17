pipeline {
    agent any

    stages {

        stage('Cloner le dépôt') {
            steps {
                git branch: 'main', url: 'https://github.com/hidara32-del/todo-app.git'
            }
        }

        stage('Build Maven') {
            steps {
                bat 'mvnw.cmd clean package'
            }
        }

        stage('Construire l image Docker') {
            steps {
                bat '"C:\\Users\\lenov\\AppData\\Local\\Programs\\DockerDesktop\\resources\\bin\\docker.exe" build -t sarahida/todo-app:latest .'
            }
        }

        stage('Push Docker Hub') {
            steps {
                bat '"C:\\Users\\lenov\\AppData\\Local\\Programs\\DockerDesktop\\resources\\bin\\docker.exe" push sarahida/todo-app:latest'
            }
        }

        stage('Supprimer ancien conteneur') {
            steps {
                bat '"C:\\Users\\lenov\\AppData\\Local\\Programs\\DockerDesktop\\resources\\bin\\docker.exe" rm -f todo-container || exit 0'
            }
        }

        stage('Lancer le conteneur') {
            steps {
                bat '"C:\\Users\\lenov\\AppData\\Local\\Programs\\DockerDesktop\\resources\\bin\\docker.exe" run -d --name todo-container -p 8081:8080 sarahida/todo-app:latest'
            }
        }

        stage('Déploiement Kubernetes') {
            steps {
                bat 'kubectl apply -f kubernetes/deployment.yaml'
                bat 'kubectl apply -f kubernetes/service.yaml'
            }
        }
    }
}