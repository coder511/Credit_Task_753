pipeline {

    agent any
 
    environment {

        RECIPIENTS = 'vanshikakaul10@gmail.com'  // Replace with the email to notify

    }
 
    stages {

        stage('Checkout') {

            steps {

                git branch: 'main', url: 'https://github.com/coder511/8.1-DevSecOps.git'

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

            post {

                always {

                    emailext attachLog: true,

                             subject: "Test Stage - Build ${currentBuild.fullDisplayName}",

                             body: "The test stage completed. Please check the attached log.",

                             to: "${RECIPIENTS}"

                }

            }

        }

        stage('NPM Audit (Security Scan)') {

            steps {

                sh 'npm audit || true'

            }

            post {

                always {

                    emailext attachLog: true,

                             subject: "NPM Audit Stage - Build ${currentBuild.fullDisplayName}",

                             body: "The npm audit stage completed. Please check the attached log.",

                             to: "${RECIPIENTS}"

                }

            }

        }

    }

}

 