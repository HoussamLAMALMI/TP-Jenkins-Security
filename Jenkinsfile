pipeline {
    agent any
    stages {
        stage('Clone Repository') {
            steps {
                // L'étape dazet mzyan b l'branch main
                git branch: 'main', url: 'https://github.com/HoussamLAMALMI/TP-Jenkins-Security.git'
            }
        }
        stage('Install Dependencies') {
            steps {
                // L'7el dyal l'erreur zednah hna: --break-system-packages
                sh 'pip install -r requirements.txt --break-system-packages'
            }
        }
        stage ('Run Tests') {
            steps {
                // L'exécution dyal les tests [cite: 48-52]
                sh 'pytest'
            }
        }
        stage('SAST Scan') {
            steps {
                // L'analyse statique [cite: 78-82]
                sh 'sonar-scanner'
            }
        }
        stage('SCA Scan') {
            steps {
                // L'analyse SCA b failOnCVSS [cite: 85-88]
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
