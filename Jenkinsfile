# Go to your repo folder
cd ~/Desktop/nodejs-goof   # or wherever you cloned it

# Edit Jenkinsfile (overwrite with new content)
cat <<'EOL' > Jenkinsfile
pipeline {
    agent any
    environment {
        PATH = "/opt/homebrew/bin:${env.PATH}"
    }
    stages {
        stage('Checkout') {
            steps {
                git branch: 'main', url: 'https://github.com/kanvirp/8.2CDevSecOps.git', credentialsId: 'github-token'
            }
        }
        stage('Install Dependencies') {
            steps {
                sh 'npm install'
            }
        }
        stage('Run Tests') {
            steps {
                sh 'npm test || true'
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
        }
    }
}
EOL

      
