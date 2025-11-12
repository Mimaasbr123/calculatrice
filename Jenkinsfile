pipeline {
    agent any

    stages {
        stage('Compilation') {
            steps {
                sh 'chmod +x gradlew'
                sh './gradlew clean compileJava'
            }
        }

        stage('Tests unitaires') {
            steps {
                sh './gradlew test'
            }
        }

        stage('Couverture de code') {
            steps {
                // Generate JaCoCo report
                sh './gradlew jacocoTestReport'

                // Publish JaCoCo HTML report in Jenkins UI
                publishHTML(target: [
                    reportDir: 'build/reports/jacoco/test/html',
                    reportFiles: 'index.html',
                    reportName: 'Rapport JaCoCo',
                    keepAll: true,
                    alwaysLinkToLastBuild: true,
                    allowMissing: false
                ])
            }
        }
    }

    post {
        always {
            // Always publish test results (JUnit)
            junit allowEmptyResults: true, testResults: 'build/test-results/test/*.xml'
        }
    }
}
