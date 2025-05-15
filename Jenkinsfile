pipeline {
    agent any

    stages {
        stage('Checkout') {
            steps {
                git branch: 'main', url: 'https://github.com/avishkaabeyrathna/Jenkins-demo.git'
            }
        }

        stage('Install Python Dependencies') {
            steps {
                script {
                    def result = bat(script: 'pip install -r requirements.txt', returnStatus: true)
                    if (result != 0) {
                        echo "No requirements.txt found — skipping dependency install."
                    }
                }
            }
        }

        stage('Run Python Script') {
            steps {
                script {
                    try {
                        bat 'python hello.py' // Replace with your actual Python file
                        currentBuild.result = 'SUCCESS'
                    } catch (err) {
                        currentBuild.result = 'FAILURE'
                        throw err
                    } finally {
                        emailext(
                            to: 'youremail@gmail.com',
                            subject: "Build ${currentBuild.fullDisplayName} - ${currentBuild.result}",
                            body: "Build finished",
                            attachLog: true,
                            mimeType: 'text/html',
                            replyTo: 'youremail@gmail.com',
                            recipientProviders: [[$class: 'DevelopersRecipientProvider']],
                            wait: true
                        )
                    }
                }
            }
        }

        stage('Security Placeholder') {
            steps {
                script {
                    bat 'echo "Security scan placeholder"'
                    emailext(
                        to: 'avishkaa342@gmail.com',
                        subject: "SECURITY SCAN - Placeholder",
                        body: "Security scan stage executed (no tool configured).",
                        attachLog: true
                    )
                }
            }
        }
    }
}
