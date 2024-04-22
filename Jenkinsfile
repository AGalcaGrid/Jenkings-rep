pipeline {
    agent any
    environment {
        PATH = "/Users/agalca/.nvm/versions/node/v14.21.3/bin:$PATH"
    }
    stages {
        stage('Install dependencies') {
            steps {
                sh 'npm install'
            }
        }
        stage('Test') {
            steps {
                sh 'npx playwright test'
            }
        }
    }
    post {
        always {
            script {
                emailext(
                    attachLog: true,
                    to: 'agalca@griddynamics.com',
                    subject: 'Automation Testing Report',
                    body: 'Please find the Playwright report attached for your reference.',
                    attachmentsPattern: '**/playwright-report/index.html'
                )
                publishHTML(target: [
                    allowMissing: true,
                    alwaysLinkToLastBuild: true,
                    keepAll: true,
                    reportDir: './playwright-report',
                    reportFiles: 'index.html',
                    reportName: 'Playwright Report',
                    reportTitles: 'Playwright Report',
                ]
                )
            }
        }
    }
}
