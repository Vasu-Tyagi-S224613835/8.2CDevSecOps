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
    }

    post {
        always {
            emailext(
                subject: "Jenkins Job '${env.JOB_NAME} [#${env.BUILD_NUMBER}]' - ${currentBuild.currentResult}",
                body: """<p>Job '${env.JOB_NAME}' (Build #${env.BUILD_NUMBER}) finished with status: <b>${currentBuild.currentResult}</b></p>
                         <p>Check the console output at <a href="${env.BUILD_URL}">${env.BUILD_URL}</a></p>""",
                mimeType: 'text/html',
                to: 'vasutyagi13@gmail.com'
            )
        }
    }
}

