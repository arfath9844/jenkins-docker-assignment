pipeline {
    agent any

    stages {

        stage('Checkout') {
            steps {
                echo 'Pulling source code'
            }
        }

        stage('Build Docker Image') {
            steps {
                sh 'docker build -t task-tracker-app .'
            }
        }

        stage('Stop Old Container') {
            steps {
                sh 'docker stop task-tracker-container || true'
                sh 'docker rm task-tracker-container || true'
            }
        }

        stage('Deploy Container') {
            steps {
                sh 'docker run -d --name task-tracker-container -p 3000:3000 task-tracker-app'
            }
        }
    }
}
