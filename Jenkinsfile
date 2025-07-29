pipeline {
    agent any

    tools {
        nodejs 'NodeJS'
    }

    stages {
        stage('Checkout') {
            steps {
                checkout scm
            }
        }

        stage('Build') {
            steps {
                echo "Build running on branch: ${env.GIT_BRANCH}"
                sh 'echo "Pretend to build..."'
            }
        }

        stage('Test') {
            steps {
                sh 'npm install'
                sh 'npm test'
            }
        }

        stage('Deploy to Dev') {
            when {
                expression { env.GIT_BRANCH == 'origin/dev' }
            }
            steps {
                echo "Deploying to Dev server..."
                // sh 'your-deploy-script-dev.sh'
            }
        }

        stage('Deploy to QA') {
            when {
                expression { env.GIT_BRANCH == 'origin/qa' }
            }
            steps {
                echo "Deploying to QA server..."
                // sh 'your-deploy-script-qa.sh'
            }
        }

        stage('Deploy to Production') {
            when {
                expression { env.GIT_BRANCH == 'origin/main' }
            }
            steps {
                echo "Deploying to Production server..."
                // sh 'your-deploy-script-prod.sh'
            }
        }
    }
}