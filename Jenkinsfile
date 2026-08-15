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
                bat '"C:\\Users\\lenov\\AppData\\Local\\Programs\\DockerDesktop\\resources\\bin\\docker.exe" build -t todo-app .'
            }
        }

        stage('Supprimer ancien conteneur') {
            steps {
                bat '"C:\\Users\\lenov\\AppData\\Local\\Programs\\DockerDesktop\\resources\\bin\\docker.exe" rm -f todo-container || exit 0'
            }
        }

        stage('Lancer le conteneur') {
            steps {
                bat '"C:\\Users\\lenov\\AppData\\Local\\Programs\\DockerDesktop\\resources\\bin\\docker.exe" run -d --name todo-container -p 8081:8080 todo-app'
            }
        }
    }
}