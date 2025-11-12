pipeline {
    agent any

    environment {
        JAVA_HOME = '/usr/lib/jvm/java-21-openjdk-amd64'
        PATH = "${JAVA_HOME}/bin:${PATH}"
    }

    stages {
        stage('Checkout') {
            steps {
                checkout scm
            }
        }

        stage('Build + Tests') {
            steps {
                sh '''
                    echo "=== Java version ==="
                    java -version
                    chmod +x gradlew
                    ./gradlew clean test
                '''
            }
            post {
                always {
                    junit allowEmptyResults: true, testResults: 'build/test-results/test/**/*.xml'
                }
            }
        }

        stage('Génération JaCoCo') {
            steps {
                sh './gradlew jacocoTestReport'
            }
        }

        stage('Analyse statique du code') {
            steps {
                sh './gradlew checkstyleMain'
                publishHTML(target: [
                    reportDir: 'build/reports/checkstyle/',
                    reportFiles: 'main.html',
                    reportName: 'Checkstyle Report'
                ])
            }
        }
    }

    post {
        always {
            // Analyse JaCoCo pour afficher la couverture dans Jenkins
            jacoco(
                execPattern: '**/jacoco.exec',
                classPattern: 'build/classes/java/main',
                sourcePattern: 'src/main/java',
                exclusionPattern: ''
            )

            // Publication du rapport HTML JaCoCo
            publishHTML(target: [
                reportDir: 'build/reports/jacoco/test/html',
                reportFiles: 'index.html',
                reportName: 'Rapport JaCoCo HTML'
            ])
        }
    }
}
