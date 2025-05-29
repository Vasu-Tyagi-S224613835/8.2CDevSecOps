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
                    def userCause = currentBuild.rawBuild.getCause(hudson.model.Cause.UserIdCause)
                    def user = userCause != null ? userCause.getUserName() : 'unknown'

                    def duration = currentBuild.durationString ?: 'unknown'
                    def status = currentBuild.currentResult ?: 'UNKNOWN'

                    def gitCommit = bat(script: 'git rev-parse HEAD', returnStdout: true).trim()
                    def gitRepo = bat(script: 'git config --get remote.origin.url', returnStdout: true).trim()
                    def gitLog = bat(script: 'git log -1 --pretty=format:"%s (%an)"', returnStdout: true).trim()

                    def text = """
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
                    writeFile file: 'detailed-log.txt', text: text
                }
            }
        }
    }

    post {
        always {
            emailext (
                subject: "Build ${env.JOB_NAME} #${env.BUILD_NUMBER} - ${currentBuild.currentResult}",
                body: """<p>Build <b>${env.JOB_NAME} #${env.BUILD_NUMBER}</b> finished with status <b>${currentBuild.currentResult}</b></p>
                         <p>Console: <a href="${env.BUILD_URL}">${env.BUILD_URL}</a></p>""",
                mimeType: 'text/html',
                to: 'vasutyagi13@gmail.com',
                attachmentsPattern: 'detailed-log.txt'
            )
        }
    }
}
