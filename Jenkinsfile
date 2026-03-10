pipeline {
    agent any
    stages {
        stage('Clone Repository') {
            steps {
                git 'https://github.com/HoussamLAMALMI/TP-Jenkins-Security.git'
            }
        }
        stage('Install Dependencies') {
            steps {
                sh 'pip install -r requirements.txt'
            }
        }
        stage ('Run Tests') {
            steps {
                sh 'pytest'
            }
        }
        stage('SAST Scan') {
            steps {
                // L'analyse statique b SonarQube kima matloub f l'étape 9
                sh 'sonar-scanner'
            }
        }
        stage('SCA Scan') {
            steps {
                // L'analyse dyal les dépendances b OWASP Dependency-Check (Étape 10)
                // W drna --failOnCVSS 7 bach ybloqui l'build kima matloub f l'étape 11
                sh 'dependency-check.sh --project "TP-Jenkins" --scan . --format HTML --failOnCVSS 7'
            }
        }
    }
    post {
        failure {
            echo 'Build failed due to errors or vulnerabilities'
        }
    }
}
