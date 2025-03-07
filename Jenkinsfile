pipeline {
    agent any

    environment {
        SONAR_HOST_URL = 'http://localhost:9000'
        SONAR_TOKEN = credentials('SONAR_TOKEN') // Remplace par l'ID du credential SonarQube dans Jenkins
    }

    stages {
        stage('Checkout') {
            steps {
                git 'https://github.com/Jacobkitams/Myhospital.git'
            }
        }

        stage('Build') {
            steps {
                 sh './gradlew build'
            }
        }

        stage('SonarQube Analysis') {
            steps {
                withSonarQubeEnv('SonarQube') {
                    sh "./gradlew sonarqube -Dsonar.login=${SONAR_TOKEN
                }
            }
        }

        stage('Quality Gate') {
            steps {
                script {
                    def qg = waitForQualityGate()
                    if (qg.status != 'OK') {
                        error "Pipeline failed due to quality gate failure: ${qg.status}"
                    }
                }
            }
        }
    }
}
