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
                bat 'mvn clean package'
            }
        }

        stage('Construire l image Docker') {
            steps {
                bat 'docker build -t todo-app .'
            }
        }

        stage('Supprimer ancien conteneur') {
            steps {
                bat 'docker rm -f todo-container || exit 0'
            }
        }

        stage('Lancer le conteneur') {
            steps {
                bat 'docker run -d --name todo-container -p 8081:8080 todo-app'
            }
        }
    }
}