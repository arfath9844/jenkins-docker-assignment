pipeline {
    agent any

    stages {

        stage('Checkout') {
            steps {
                checkout scm
            }
        }

        stage('Install Dependencies') {
            steps {
                sh 'npm install'
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

        stage('Deploy') {
            steps {
                sh 'docker run -d --name task-tracker-container -p 3000:3000 task-tracker-app'
            }
        }

        stage('Health Check') {
            steps {
                sh 'sleep 10'
                sh 'curl http://localhost:3000/health'
            }
        }

        stage('Cleanup') {
            steps {
                sh 'docker image prune -f'
            }
        }
    }

    post {
        success {
            echo 'Pipeline executed successfully!'
        }

        failure {
            echo 'Pipeline failed. Check the console output.'
        }

        always {
            echo 'Pipeline execution completed.'
        }
    }
}
