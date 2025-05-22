pipeline {
    agent any

    environment {
        EMAIL_RECIPIENT = 'sanjanaraha46@gmail.com'
    }

    stages {
        stage('Checkout') {
            steps {
                git branch: 'main', url: 'https://github.com/Sanjana-46/SIT753-CI-Pipeline.git'
            }
        }

        stage('Install Dependencies') {
            steps {
                sh 'npm install'
            }
        }

        stage('Run Tests') {
            steps {
                script {
                    try {
                        sh 'npm test'
                        currentBuild.result = 'SUCCESS'
                    } catch (e) {
                        currentBuild.result = 'FAILURE'
                        throw e
                    }
                }
            }
            post {
                always {
                    emailext (
                        subject: "Run Tests: ${currentBuild.result}",
                        body: "Test Stage completed: ${currentBuild.result}. Log: ${env.BUILD_URL}",
                        to: "${EMAIL_RECIPIENT}",
                        attachLog: true
                    )
                }
            }
        }

        stage('Security Scan') {
            steps {
                script {
                    try {
                        sh 'npm audit'
                        currentBuild.result = 'SUCCESS'
                    } catch (e) {
                        currentBuild.result = 'FAILURE'
                        throw e
                    }
                }
            }
            post {
                always {
                    emailext (
                        subject: "Security Scan: ${currentBuild.result}",
                        body: "Security Scan completed: ${currentBuild.result}. Log: ${env.BUILD_URL}",
                        to: "${EMAIL_RECIPIENT}",
                        attachLog: true
                    )
                }
            }
        }
    }
}
