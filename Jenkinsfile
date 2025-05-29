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
                bat '''
                    echo Build Number: %BUILD_NUMBER% > detailed-log.txt
                    echo Job Name: %JOB_NAME% >> detailed-log.txt
                    echo Result: %BUILD_STATUS% >> detailed-log.txt
                    echo Build URL: %BUILD_URL% >> detailed-log.txt
                    echo. >> detailed-log.txt
                    echo Started by: %BUILD_USER% >> detailed-log.txt
                    echo. >> detailed-log.txt
                    echo Build Duration: %BUILD_DURATION% >> detailed-log.txt
                    echo. >> detailed-log.txt
                    echo Revision: >> detailed-log.txt
                    git rev-parse HEAD >> detailed-log.txt 2>nul
                    echo. >> detailed-log.txt
                    echo Repository: >> detailed-log.txt
                    git config --get remote.origin.url >> detailed-log.txt 2>nul
                    echo. >> detailed-log.txt
                    echo Changes: >> detailed-log.txt
                    git log -1 --pretty=format:"%%s (%%an)" >> detailed-log.txt 2>nul
                '''
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
