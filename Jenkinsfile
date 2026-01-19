pipeline {
    agent any

    tools {
        nodejs 'nodejs-25'
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
                sh 'npx playwright install --with-deps'
            }
        }

        stage('Run Playwright Tests') {
            steps {
                sh 'npx playwright test'
            }
        }
    }

    post {
        always {
            
            publishHTML(target: [
                alwaysLinkToLastBuild: true,
                keepAll: true,
                reportDir: 'playwright-report',
                reportFiles: 'index.html',
                reportName: 'Playwright HTML Report'
            ])

            
            emailext (
                subject: "Status: ${currentBuild.result ?: 'SUCCESS'} - Job: ${env.JOB_NAME} [Build #${env.BUILD_NUMBER}]",
                body: "The test run is complete. You can view the full report here: ${env.BUILD_URL}Playwright_20HTML_20Report/",
                to: 'pradeeshannagarajan@gmail.com',
                mimeType: 'text/html'
            )
        }
    }
}