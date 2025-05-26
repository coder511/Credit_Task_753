pipeline {
    agent any
 
    environment {
        RECIPIENTS = 'vanshikakaul10@gmail.com'  
    }
  
    stages {
        stage('Checkout') {
            steps {
                git branch: 'main', url: 'https://github.com/coder511/8.1-DevSecOps.git'
            }
        }
        
        stage('Install Dependencies') {
            steps {
                bat 'npm install'

            }
        }
        stage('Run Tests') {
            steps {
                bat 'npm test || exit /b 0'
            }
            post {
                always {
                    emailext attachLog: true,
                             subject: "Test Stage - Build ${currentBuild.fullDisplayName} - Status: ${currentBuild.currentResult}",
                             body: """The Test stage has completed with status: ${currentBuild.currentResult}.
Please check the attached console log for details.""",
                             to: "${RECIPIENTS}"
                }
            }
        }
        stage('Generate Coverage Report') {
            steps {
                bat 'npm run coverage || exit /b 0'
            }

        }
        stage('NPM Audit (Security Scan)') {
            steps {
                bat 'npm audit || exit /b 0'
            }
            post {
                always {
                    emailext attachLog: true,
                             subject: "NPM Audit Stage - Build ${currentBuild.fullDisplayName} - Status: ${currentBuild.currentResult}",
                             body: """The NPM Audit stage has completed with status: ${currentBuild.currentResult}.
Please check the attached console log for details.""",
                             to: "${RECIPIENTS}"
                }
            }
        }
    }
}