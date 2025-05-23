pipeline {
    agent any

    environment {
        STUDENT_ID = 's224613835'
        STUDENT_NAME = 'Vasu Tyagi'
        STAGING_SERVER = 'Azure_staging_level.gmail.com'
        PRODUCTION_SERVER = 's224613835.com'
        RECIPIENT_EMAIL = 'vasutyagi13@gmail.com'
    }

    stages {
        stage('Build') {
            steps {
                echo "${STUDENT_ID} - ${STUDENT_NAME}"
                echo 'This pipeline is in the building stage'
                bat 'echo .\\gradlew clean assemble'
            }
        }

        stage('Unit and Integration Tests') {
            steps {
                echo "${STUDENT_ID} - ${STUDENT_NAME}"
                echo 'Running unit and integration tests'
                bat 'echo Using Spock for unit tests, JMockit for integration tests'
            }
            post {
                always {
                    emailext(
                        subject: "Jenkins Build by ${STUDENT_ID} (${STUDENT_NAME}) #${BUILD_NUMBER} - Tests: ${currentBuild.currentResult}",
                        body: "Hi,\n\nThe test stage of the Jenkins pipeline executed by ${STUDENT_ID} (${STUDENT_NAME}) has completed.\n\nPlease see the attached logs for more details.\n\nThanks,\nJenkins",
                        to: "${RECIPIENT_EMAIL}",
                        attachLog: true
                    )
                }
            }
        }

        stage('Code Analysis') {
            steps {
                echo "${STUDENT_ID} - ${STUDENT_NAME}"
                echo 'Performing code analysis'
                bat 'echo Using CodeClimate for static analysis'
            }
        }

        stage('Security Scan') {
            steps {
                echo "${STUDENT_ID} - ${STUDENT_NAME}"
                echo 'Performing security scan'
                bat 'echo Using Burp Suite for scanning'
            }
            post {
                always {
                    emailext(
                        subject: "Jenkins Build by ${STUDENT_ID} (${STUDENT_NAME}) #${BUILD_NUMBER} - Security Scan Status: ${currentBuild.currentResult}",
                        body: "Hi,\n\nThe security scan stage completed for pipeline run by ${STUDENT_ID} (${STUDENT_NAME}).\n\nPlease find the attached logs for more information.\n\nRegards,\nJenkins",
                        to: "${RECIPIENT_EMAIL}",
                        attachLog: true
                    )
                }
            }
        }

        stage('Deploy to Staging') {
            steps {
                echo "${STUDENT_ID} - ${STUDENT_NAME}"
                echo 'Deploying to staging environment'
                bat "echo Deploying to ${STAGING_SERVER}"
            }
        }

        stage('Integration Tests on Staging') {
            steps {
                echo "${STUDENT_ID} - ${STUDENT_NAME}"
                echo 'Running integration tests on staging server'
                bat 'echo Executing staging integration tests...'
            }
        }

        stage('Deploy to Production') {
            steps {
                echo "${STUDENT_ID} - ${STUDENT_NAME}"
                echo 'Deploying to production environment'
                bat "echo Deploying to ${PRODUCTION_SERVER}"
            }
        }
    }

    post {
        failure {
            emailext(
                subject: "Jenkins Build by ${STUDENT_ID} (${STUDENT_NAME}) #${BUILD_NUMBER} - Failed",
                body: "Hi,\n\nThe Jenkins pipeline run by ${STUDENT_ID} (${STUDENT_NAME}) has failed.\n\nPlease check the attached Jenkins logs to debug the issue.\n\nThanks,\nJenkins",
                to: "${RECIPIENT_EMAIL}",
                attachLog: true
            )
        }
    }
}
