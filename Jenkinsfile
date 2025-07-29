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
            // Không build nếu là PR vào nhánh khác
            when {
                not {
                    changeRequest()
                }
            }
            steps {
                echo "Build running on branch: ${env.BRANCH_NAME}"
                sh 'echo "Pretend to build..."'
            }
        }

        stage('Test') {
            // Không test nếu là PR vào nhánh khác
            when {
                not {
                    changeRequest()
                }
            }
            steps {
                sh 'npm install'
                sh 'npm test'
            }
        }

        stage('Deploy to Dev') {
            when {
                allOf {
                    branch 'dev'
                    not { changeRequest() } // Không chạy nếu dev là source của PR
                }
            }
            steps {
                echo "Deploying to Dev server..."
                // script here
            }
        }

        stage('Deploy to QA') {
            when {
                allOf {
                    branch 'qa'
                    not { changeRequest() }
                }
            }
            steps {
                echo "Deploying to QA server..."
                // script here
            }
        }

        stage('Deploy to Production') {
            when {
                allOf {
                    branch 'main'
                    not { changeRequest() }
                }
            }
            steps {
                echo "Deploying to Production server..."
                // script here
            }
        }

        stage('Handle Pull Requests') {
            when {
                changeRequest()
            }
            steps {
                echo "🚀 This is a PR from ${env.CHANGE_BRANCH} into ${env.CHANGE_TARGET}"
            }
        }
    }
}
