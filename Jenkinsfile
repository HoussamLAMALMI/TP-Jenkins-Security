pipeline {
    agent any
    environment {
        SONAR_HOME = tool('SonarScanner')
        DP_HOME = tool('DP-Check')
        // Kan-configuriw l'PATH bach ychouf ga3 les dossiers dyal les outils
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
                catchError(buildResult: 'SUCCESS', stageResult: 'FAILURE') {
                    sh 'sonar-scanner -Dsonar.projectKey=TP-Jenkins -Dsonar.sources=.'
                }
            }
        }
        stage('SCA Scan') {
            steps {
                // Beddelna l'smiya dyal l'commande l "dependency-check"
                sh 'dependency-check --project "TP-Jenkins" --scan . --format HTML --failOnCVSS 7'
            }
        }
    }
    post {
        failure {
            echo 'Build failed due to errors or vulnerabilities'
        }
    }
}
