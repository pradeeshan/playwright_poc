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
            script {
                // This line relaxes Jenkins security so the HTML report styles and JS load correctly
                System.setProperty("hudson.model.DirectoryBrowserSupport.CSP", "")
            }
            
            echo 'Publishing Playwright HTML Report...'
            
            publishHTML(target: [
                alwaysLinkToLastBuild: true,
                keepAll: true,
                reportDir: 'playwright-report',
                reportFiles: 'index.html',
                reportName: 'Playwright HTML Report'
            ])
        }
    }
}
