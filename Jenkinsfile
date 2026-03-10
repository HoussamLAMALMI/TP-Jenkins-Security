pipeline {
    agent any
    
    // Had l'bloc ghadi ykhli Jenkins y-ajouter les outils l'PATH automatique
    tools {
        sonarRunner 'SonarScanner'
        dependency-check 'DP-Check'
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
                    // Daba n9drou n-lanciwha direct 7it Jenkins 3ref l'PATH
                    sh 'sonar-scanner -Dsonar.projectKey=TP-Jenkins -Dsonar.sources=.'
                }
            }
        }
        stage('SCA Scan') {
            steps {
                // Smiya s7i7a f Linux hiya dependency-check.sh
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
