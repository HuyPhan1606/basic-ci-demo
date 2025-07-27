pipeline {
    agent any

    environment {
        DISABLE_LOGGING = 'true'
    }

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

        stage('Deploy (Simulated)') {
            steps {
                echo 'ssh user@your-vps-ip "cd /app && git pull && pm2 restart app"'
            }
        }
    }
}
