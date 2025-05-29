pipeline {
    agent any

    environment {
        BUILD_USER = "${env.BUILD_USER ?: 'unknown'}"
    }

    stages {
        stage('Build') {
            steps {
                echo 'Running build...'
                // Simulate build step
            }
        }

        stage('Test') {
            steps {
                echo 'Running tests...'
                bat 'echo All tests passed. > test-results.log'
            }
        }

        stage('Security Scan') {
            steps {
                echo 'Running security scan...'
                bat 'echo No issues found. > security-scan.log'
            }
        }

        stage('Generate Detailed Log') {
            steps {
                script {
                    def user = env.BUILD_USER ?: 'unknown'
                    def status = currentBuild.currentResult ?: 'UNKNOWN'
                    def duration = currentBuild.durationString ?: 'unknown'

                    def gitCommit = bat(script: 'git rev-parse HEAD', returnStdout: true).trim()
                    def gitRepo = bat(script: 'git config --get remote.origin.url', returnStdout: true).trim()
                    def gitLog = bat(script: 'git log -1 --pretty=format:"%s (%an)"', returnStdout: true).trim()

                    def detailedLog = """
Build Number: ${env.BUILD_NUMBER}
Job Name: ${env.JOB_NAME}
Status: ${status}
Build URL: ${env.BUILD_URL}

Started by: ${user}

Build Duration: ${duration}

Revision:
${gitCommit}

Repository:
${gitRepo}

Changes:
${gitLog}
"""

                    // Write detailed log to result.log file
                    writeFile file: 'result.log', text: detailedLog

                    // Save to env var to reuse in email body
                    env.DETAILED_LOG_BODY = detailedLog.replace('\n', '<br>')
                }
            }
        }
    }

    post {
        always {
            emailext (
                subject: "Build ${env.JOB_NAME} #${env.BUILD_NUMBER} - ${currentBuild.currentResult}",
                body: """<p>Build <b>${env.JOB_NAME} #${env.BUILD_NUMBER}</b> finished with status <b>${currentBuild.currentResult}</b>.</p>
                         <p>Please find the attached detailed build log file.</p>
                         <p><b>Detailed Log:</b><br>${env.DETAILED_LOG_BODY}</p>
                         <p>Console Output: <a href="${env.BUILD_URL}">${env.BUILD_URL}</a></p>""",
                mimeType: 'text/html',
                to: 'vasutyagi13@gmail.com',
                attachmentsPattern: 'result.log'
            )
        }
    }
}
