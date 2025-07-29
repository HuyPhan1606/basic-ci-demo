pipeline {
    agent any

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
    }
}
