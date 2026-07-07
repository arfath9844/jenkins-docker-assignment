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

        stage('Run Tests') {
            steps {
                sh 'npm test'
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

        stage('Health Check') {
    steps {
        sh '''
        sleep 10
        node -e "
        const http = require('http');
        http.get('http://172.17.0.1:3000/health', (res) => {
            if (res.statusCode !== 200) process.exit(1);
            console.log('Health check passed');
        }).on('error', () => process.exit(1));
        "
        '''
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
            echo 'Pipeline failed!'
        }
        always {
            echo 'Pipeline execution completed.'
        }
    }
}
