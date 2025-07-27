pipeline {
    agent any

    tools {
        nodejs 'NodeJS' // 👈 This name must match exactly with the one in the plugin config
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
                echo 'ssh user@your-vps "cd /app && git pull && pm2 restart app"'
            }
        }
    }
}
