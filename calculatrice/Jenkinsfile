pipeline {
    agent any

    stages {
        stage('Checkout') {
            steps {
                git url: 'https://github.com/Mimaasbr123/calculatrice.git', branch: 'main'
            }
        }

        stage('Compilation') {
            steps {
                dir('calculatrice') {
                    sh './gradlew clean compileJava'
                }
            }
        }

        stage('Tests unitaires') {
            steps {
                dir('calculatrice') {
                    sh './gradlew test'
                }
            }
        }
    }

    
}
