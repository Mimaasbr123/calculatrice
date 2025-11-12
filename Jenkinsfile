pipeline {
    agent any

    stages {
        stage('Checkout') {
            steps {
                // Clone the repository directly into Jenkins workspace
                git url: 'https://github.com/Mimaasbr123/calculatrice.git', branch: 'main'
            }
        }

        stage('Compilation') {
            steps {
                // Ensure gradlew is executable, then compile
                sh 'chmod +x gradlew'
                sh './gradlew clean compileJava'
            }
        }

        stage('Tests unitaires') {
            steps {
                sh './gradlew test'
            }
        }
    }

    post {
        always {
            // Publish JUnit test results if available
            junit allowEmptyResults: true, testResults: 'build/test-results/test/*.xml'
        }
    }
}
