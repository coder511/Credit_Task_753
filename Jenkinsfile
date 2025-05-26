pipeline {
    agent any

    stages {
        stage('Checkout') {
            steps {
                git branch: 'main', url: 'https://github.com/coder511/Credit_Task_753.git'
            }
        }

        stage('Install Dependencies') {
            steps {
                bat 'npm install'
            }
        }

        stage('Run Tests') {
            steps { 
                bat 'cmd /c "npm test || exit /b 0"' // Allows continuation on failure
            }
        }

        stage('Generate Coverage Report') {
            steps {
                bat 'cmd /c "npm run coverage || exit /b 0"'
            }
        }

        stage('NPM Audit (Security Scan)') {
            steps {
                bat 'cmd /c "npm audit || exit /b 0"'
            }
        }

        stage('SonarCloud Analysis') {
            steps {
                withCredentials([string(credentialsId: 'SONAR_TOKEN', variable: 'SONAR_TOKEN')]) {
                    bat '''
                        curl -LO https://binaries.sonarsource.com/Distribution/sonar-scanner-cli/sonar-scanner-cli-5.0.1.3006-windows.zip
                        powershell -Command "Expand-Archive -Force sonar-scanner-cli-5.0.1.3006-windows.zip ."
                        set PATH=%PATH%;%CD%\\sonar-scanner-5.0.1.3006-windows\\bin

                        sonar-scanner ^
                          -Dsonar.login=%SONAR_TOKEN%
                    '''
                }
            }
        }
    }
}
