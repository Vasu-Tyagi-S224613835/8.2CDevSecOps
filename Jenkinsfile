pipeline {
    agent any

    stages {
        stage('Build') {
            steps {
                echo 'Running build...'
                // Add actual build steps here
            }
        }

        stage('Test') {
            steps {
                echo 'Running tests...'
                // Simulate test log creation
                bat 'echo "Test Logs: All tests passed." > test-results.log'
            }
            post {
                always {
                    emailext (
                        subject: "Test Stage: ${currentBuild.currentResult}",
                        body: "Test stage finished with status: ${currentBuild.currentResult}",
                        to: 'vasutyagi13@gmail.com',
                        attachmentsPattern: 'test-results.log',
                        mimeType: 'text/plain'
                    )
                }
            }
        }

        stage('Security Scan') {
            steps {
                echo 'Running security scan...'
                // Simulate security scan log
                bat 'echo "Security scan completed with no issues." > security-scan.log'
            }
            post {
                always {
                    emailext (
                        subject: "Security Scan Stage: ${currentBuild.currentResult}",
                        body: "Security scan finished with status: ${currentBuild.currentResult}",
                        to: 'vasutyagi13@gmail.com',
                        attachmentsPattern: 'security-scan.log',
                        mimeType: 'text/plain'
                    )
                }
            }
        }
    }
}
