pipeline {
    agent any
    stages {
        stage('Checkout') {
            steps {
                git branch: 'main', url: 'https://github.com/avishkaabeyrathna/Jenkins-demo.git/'
            }
        }
        stage('Install Dependencies') {
            steps {
                bat 'npm install'
            }
        }
        stage('Run Tests') {
            steps {
                script {
                    try {
                        bat 'npm test'
                        currentBuild.result = 'SUCCESS'
                    } catch (err) {
                        currentBuild.result = 'FAILURE'
                        throw err
                    } finally {
                        emailext(
                            to: 'youremail@gmail.com',
                            subject: "TEST STAGE - ${currentBuild.result}",
                            body: "The TEST stage of the pipeline finished with status: ${currentBuild.result}",
                            attachLog: true
                        )
                    }
                }
            }
        }
        stage('Generate Coverage') {
            steps {
                bat 'npm run coverage || exit /b 0'
            }
        }
        stage('Security Scan') {
            steps {
                script {
                    try {
                        bat 'npm audit'
                        currentBuild.result = 'SUCCESS'
                    } catch (err) {
                        currentBuild.result = 'FAILURE'
                        throw err
                    } finally {
                        emailext(
                            to: 'youremail@gmail.com',
                            subject: "SECURITY SCAN - ${currentBuild.result}",
                            body: "The security scan completed with: ${currentBuild.result}",
                            attachLog: true
                        )
                    }
                }
            }
        }
    }
}
