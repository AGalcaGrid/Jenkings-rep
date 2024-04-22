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
        stage('Run tests') {
            steps {
                sh 'npx playwright test'
            }
        }
        stage('Generate Allure report') {
            steps {
                sh 'allure generate allure-results -o allure-report --clean'
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
                    body: 'Please find the Allure Playwright report attached for your reference.',
                    attachmentsPattern: '**/allure-report/**'
                )
                archiveArtifacts artifacts: 'allure-report/**', allowEmptyArchive: true
                allure([includeProperties: false, jdk: '', properties: [], reportBuildPolicy: 'ALWAYS', results: [[path: 'allure-results']]])
            }
        }
    }
}
