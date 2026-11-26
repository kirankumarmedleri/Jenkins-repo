pipeline {
    // Run on any available Jenkins agent/node
    agent any

    // Environment variables accessible across all stages
    environment {
        APP_NAME = "student-demo-app"
        APP_VERSION = "1.0.${BUILD_NUMBER}"
    }

    stages {
        // STAGE 1: CONTINUOUS INTEGRATION (Build & Lint)
        stage('1. Build & Code Check') {
            steps {
                echo "Building ${APP_NAME} version ${APP_VERSION}..."
                sh 'python3 --version'
                sh 'python3 app.py'
            }
        }

        // STAGE 2: CONTINUOUS INTEGRATION (Automated Testing)
        stage('2. Automated Unit Tests') {
            steps {
                echo "Running unit tests..."
                // Simple inline test asserting 2 + 3 == 5
                sh 'python3 -c "import app; assert app.add(2, 3) == 5, \'Test Failed!\'; print(\'All unit tests passed!\')"'
            }
        }

        // STAGE 3: CONTINUOUS DELIVERY (Package & Deploy to Staging)
        stage('3. Package & Deploy to Staging') {
            steps {
                echo "Packaging ${APP_NAME}-${APP_VERSION}.tar.gz..."
                sh "tar -czf ${APP_NAME}-${APP_VERSION}.tar.gz app.py"
                // Archive the artifact in Jenkins so students can download it from the UI
                archiveArtifacts artifacts: "*.tar.gz", fingerprint: true
                echo "Deployed to STAGING environment successfully!"
            }
        }

        // STAGE 4: CONTINUOUS DELIVERY GATE (Manual Human Approval)
        // Tip for class: Show students how the pipeline pauses here!
        // Explain that removing this 'input' block turns it into Continuous Deployment.
        stage('4. Approve Production Release') {
            steps {
                input message: 'Staging looks good! Approve deployment to Production?', ok: 'Deploy to Prod'
            }
        }

        // STAGE 5: PRODUCTION DEPLOYMENT
        stage('5. Deploy to Production') {
            steps {
                echo "Deploying ${APP_NAME} v${APP_VERSION} to LIVE PRODUCTION..."
                sleep 2
                echo "Deployment Complete! App is live."
            }
        }
    }

    // POST ACTIONS: Always run after stages finish (great for notifications/cleanup)
    post {
        success {
            echo "Pipeline SUCCEEDED! Version ${APP_VERSION} is live."
        }
        failure {
            echo "Pipeline FAILED! Alerting the dev team immediately."
        }
        always {
            echo "Cleaning up temporary workspace files..."
            cleanWs()
        }
    }
}
