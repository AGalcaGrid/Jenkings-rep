pipeline {
    agent any
    tools { nodejs "14" }
    stages {
        stage('Build') {
            steps {
                node {
                    sh 'npm install'
                }
            }
        }
        stage('Test') {
            steps {
                node {
                    sh 'npx playwright test'
                }
            }
        }
        stage('Report') {
            steps {
                node {
                    sh 'allure generate allure-results -o allure-report --clean'
                }
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
                archiveArtifacts artifacts: 'allure-report/**', allowEmptyArchive: true
                allure([includeProperties: false, jdk: '', properties: [], reportBuildPolicy: 'ALWAYS', results: [[path: 'allure-results']]])
            }
        }
    }
}
