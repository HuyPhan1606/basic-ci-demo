pipeline {
    agent any

    tools {
        nodejs 'NodeJS'
    }

    environment {
        // Xác định xem có phải PR không và target branch là gì
        IS_PR = "${env.CHANGE_ID ? 'true' : 'false'}"
        PR_TARGET = "${env.CHANGE_TARGET ?: ''}"
        CURRENT_BRANCH = "${env.BRANCH_NAME}"
    }

    stages {
        stage('Checkout') {
            steps {
                checkout scm
                script {
                    echo "🔍 Pipeline Info:"
                    echo "   - Current Branch: ${env.CURRENT_BRANCH}"
                    echo "   - Is PR: ${env.IS_PR}"
                    if (env.IS_PR == 'true') {
                        echo "   - PR: ${env.CHANGE_BRANCH} → ${env.PR_TARGET}"
                        echo "   - PR ID: ${env.CHANGE_ID}"
                    }
                }
            }
        }

        stage('Build & Test') {
            // Chỉ build khi KHÔNG phải PR (tức là push trực tiếp vào branch)
            when {
                not {
                    changeRequest()
                }
            }
            steps {
                echo "🔨 Building on branch: ${env.CURRENT_BRANCH}"
                sh 'echo "Installing dependencies..."'
                sh 'npm install'
                
                echo "🧪 Running tests..."
                sh 'npm test'
                
                echo "📦 Building application..."
                sh 'echo "Build completed successfully!"'
            }
        }

        stage('PR Validation') {
            // Chỉ chạy khi là PR - validate việc merge
            when {
                changeRequest()
            }
            steps {
                script {
                    echo "🔍 Validating PR: ${env.CHANGE_BRANCH} → ${env.PR_TARGET}"
                    
                    // Chỉ install dependencies và chạy test cơ bản để validate PR
                    echo "📥 Installing dependencies for PR validation..."
                    sh 'npm install'
                    
                    echo "🧪 Running basic tests to validate merge..."
                    sh 'npm test'
                    
                    echo "✅ PR validation completed!"
                    echo "   - Source: ${env.CHANGE_BRANCH}"
                    echo "   - Target: ${env.PR_TARGET}"
                    echo "   - Ready for merge!"
                }
            }
        }

        stage('Deploy to Dev') {
            when {
                allOf {
                    branch 'dev'
                    not { changeRequest() } // Chỉ deploy khi push trực tiếp, không phải PR
                }
            }
            steps {
                echo "🚀 Deploying to Dev server..."
                echo "   - Branch: dev"
                echo "   - Environment: Development"
                // Thêm script deploy thực tế ở đây
                sh 'echo "Deploy to dev completed!"'
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
                echo "🚀 Deploying to QA server..."
                echo "   - Branch: qa"
                echo "   - Environment: Quality Assurance"
                // Thêm script deploy thực tế ở đây
                sh 'echo "Deploy to QA completed!"'
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
                echo "🚀 Deploying to Production server..."
                echo "   - Branch: main"
                echo "   - Environment: Production"
                echo "⚠️  Production deployment - please verify!"
                // Thêm script deploy thực tế ở đây
                sh 'echo "Deploy to production completed!"'
            }
        }

        stage('Cleanup') {
            steps {
                echo "🧹 Cleaning up workspace..."
                // Cleanup nếu cần
                sh 'echo "Cleanup completed!"'
            }
        }
    }

    post {
        always {
            echo "📊 Pipeline Summary:"
            echo "   - Branch: ${env.CURRENT_BRANCH}"
            echo "   - Is PR: ${env.IS_PR}"
            echo "   - Status: ${currentBuild.currentResult}"
        }
        
        success {
            script {
                if (env.IS_PR == 'true') {
                    echo "✅ PR validation successful! Ready to merge ${env.CHANGE_BRANCH} → ${env.PR_TARGET}"
                } else {
                    echo "✅ Branch build successful on ${env.CURRENT_BRANCH}!"
                }
            }
        }
        
        failure {
            script {
                if (env.IS_PR == 'true') {
                    echo "❌ PR validation failed! Please fix issues before merging."
                } else {
                    echo "❌ Branch build failed on ${env.CURRENT_BRANCH}!"
                }
            }
        }
        
        cleanup {
            // Cleanup workspace nếu cần
            deleteDir()
        }
    }
}