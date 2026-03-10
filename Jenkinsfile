pipeline {
    agent any
    stages {
        stage('Clone Repository') {
            steps {
                git branch: 'main', url: 'https://github.com/HoussamLAMALMI/TP-Jenkins-Security.git'
            }
        }
        stage('Install Dependencies') {
            steps {
                sh 'pip install -r requirements.txt --break-system-packages'
            }
        }
        stage ('Run Tests') {
            steps {
                sh 'pytest'
            }
        }
        stage('SAST Scan') {
            tools {
                // Kan3eytou l l'outil li yallah smaynah f Jenkins
                sonarQubeScanner 'SonarScanner'
            }
            steps {
                sh 'sonar-scanner'
            }
        }
        stage('SCA Scan') {
            tools {
                // Kan3eytou l l'outil dyal OWASP li sawbna f l'étape 7 w smaynah DP-Check
                dependencyCheck 'DP-Check'
            }
            steps {
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
