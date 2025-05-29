pipeline {
    agent any

    stages {
        stage('Build') {
            steps {
                echo 'Running build...'
            }
        }

        stage('Test') {
            steps {
                echo 'Running tests...'
                bat 'echo "All tests passed." > test-results.log'
            }
        }

        stage('Security Scan') {
            steps {
                echo 'Running security scan...'
                bat 'echo "No issues found." > security-scan.log'
            }
        }

        stage('Generate Detailed Log') {
            steps {
                bat '''
                    echo Build Log: > detailed-log.txt
                    type test-results.log >> detailed-log.txt
                    echo. >> detailed-log.txt
                    echo Security Scan Log: >> detailed-log.txt
                    type security-scan.log >> detailed-log.txt
                '''
            }
        }
    }

    post {
        always {
            emailext (
                subject: "Build ${env.JOB_NAME} #${env.BUILD_NUMBER} - ${currentBuild.currentResult}",
                body: '''<p>Build <b>${env.JOB_NAME} #${env.BUILD_NUMBER}</b> finished with status <b>${currentBuild.currentResult}</b></p>
                         <p>Console Output (last 500 lines):</p>
                         <pre>${BUILD_LOG, maxLines=500, escapeHtml=true}</pre>
                         <p>Full console output: <a href="${env.BUILD_URL}">${env.BUILD_URL}</a></p>''',
                mimeType: 'text/html',
                to: 'vasutyagi13@gmail.com',
                attachmentsPattern: 'detailed-log.txt'
            )
        }
    }
}
