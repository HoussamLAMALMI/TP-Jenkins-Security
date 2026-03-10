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
            steps {
                // Khdmna b catchError bach may-bloquich le build hna [cite: 60, 62]
                catchError(buildResult: 'SUCCESS', stageResult: 'FAILURE') {
                    script {
                        def sonarScanner = tool 'SonarScanner'
                        sh "${sonarScanner}/bin/sonar-scanner -Dsonar.projectKey=TP-Jenkins -Dsonar.sources=."
                    }
                }
            }
        }
        stage('SCA Scan') {
            steps {
                script {
                    // Kanjibou l'chemin dyal l'outil s7i7 [cite: 85]
                    def dpCheck = tool 'DP-Check'
                    // L'commande s7i7a f Docker hiya li bla .sh [cite: 55]
                    sh "${dpCheck}/bin/dependency-check.sh --project 'TP-Jenkins' --scan . --format HTML --failOnCVSS 7"
                }
            }
        }
    }
    post {
        failure {
            echo 'Build failed due to errors or vulnerabilities'
        }
    }
}
