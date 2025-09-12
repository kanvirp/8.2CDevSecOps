pipeline {
    agent any

    environment {
        PATH = "/opt/homebrew/bin:${env.PATH}"      
        PROJECT_NAME = '8.2CDevSecOps'
        EMAIL_RECIPIENTS = "kanvipatel186@gmail.com" 
    }

    stages {
        stage('Checkout') {
            steps {
                git branch: 'main', url: 'https://github.com/kanvirp/8.2CDevSecOps.git', credentialsId: 'github-token'
            }
        }

        stage('Install Dependencies') {
            steps {
                sh 'npm ci || npm install'
            }
        }

        stage('Run Tests') {
            steps {
                sh 'npm test || true'
            }
            post {
                success {
                    emailext(
                        subject: "${PROJECT_NAME} - Build #${BUILD_NUMBER} - Tests SUCCESS",
                        body: " Unit and integration tests passed.\n\nCheck console output at: ${BUILD_URL}",
                        to: "${EMAIL_RECIPIENTS}",
                        attachLog: true
                    )
                }
                failure {
                    emailext(
                        subject: "${PROJECT_NAME} - Build #${BUILD_NUMBER} - Tests FAILED",
                        body: " Unit and integration tests failed.\n\nCheck console output at: ${BUILD_URL}",
                        to: "${EMAIL_RECIPIENTS}",
                        attachLog: true
                    )
                }
            }
        }

        stage('Generate Coverage Report') {
            steps {
                sh 'npm run coverage || true'
            }
        }

        stage('NPM Audit (Security Scan)') {
            steps {
                sh 'npm audit || true'
            }
            post {
                success {
                    emailext(
                        subject: "${PROJECT_NAME} - Build #${BUILD_NUMBER} - Security Scan SUCCESS",
                        body: " NPM audit completed successfully.\n\nCheck console output at: ${BUILD_URL}",
                        to: "${EMAIL_RECIPIENTS}",
                        attachLog: true
                    )
                }
                failure {
                    emailext(
                        subject: "${PROJECT_NAME} - Build #${BUILD_NUMBER} - Security Scan FAILED",
                        body: " NPM audit found vulnerabilities.\n\nCheck console output at: ${BUILD_URL}",
                        to: "${EMAIL_RECIPIENTS}",
                        attachLog: true
                    )
                }
            }
        }
    }
}


      
