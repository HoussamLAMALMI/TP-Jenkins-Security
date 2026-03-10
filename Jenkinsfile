pipeline {
    agent any
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
                // Kankhlliw Jenkins ykemmel wakha ykon erreur f Sonar
                catchError(buildResult: 'SUCCESS', stageResult: 'FAILURE') {
                    sh 'sonar-scanner -Dsonar.projectKey=TP-Jenkins -Dsonar.sources=.'
                }
            }
        }
        stage('SCA Scan') {
            steps {
                // L'analyse SCA b blocage CVSS 7 [cite: 85-88]
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
