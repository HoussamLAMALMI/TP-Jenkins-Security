pipeline {
    agent any
    
    // Hna kanjibou les outils w kanzidouhom f l'PATH dyal l'environnement
    environment {
        SONAR_HOME = tool('SonarScanner')
        DP_HOME = tool('DP-Check')
        PATH = "${SONAR_HOME}/bin:${DP_HOME}/bin:${env.PATH}"
    }

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
                // Daba ghadi yl9aha 7it zednaha f l'PATH lfo9
                sh 'sonar-scanner'
            }
        }
        stage('SCA Scan') {
            steps {
                // L'analyse dyal OWASP b blocage CVSS 7 kima matloub f l'étape 11
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
