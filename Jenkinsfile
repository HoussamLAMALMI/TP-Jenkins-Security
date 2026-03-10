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
        
        stage('Run Tests') { 
            steps { 
                sh 'pytest' 
            } 
        }
        
        stage('SAST Scan (SonarQube)') {
            steps {
                catchError(buildResult: 'SUCCESS', stageResult: 'FAILURE') {
                    script {
                        def sonarScanner = tool name: 'SonarScanner'
                        
                        // 🚨 BDL HAD 'TON_ORGANIZATION_SONAR' B SMYAT L'ORGANISATION DYALEK F SONARCLOUD
                        sh "${sonarScanner}/bin/sonar-scanner -Dsonar.organization=TON_ORGANIZATION_SONAR -Dsonar.projectKey=TP-Jenkins -Dsonar.sources=."
                    }
                }
            }
        }
        
        stage('SCA Scan (OWASP)') {
            steps {
                // Hna khdemna b l'plugin nichan bach y3ref l'chemin dyal 'DP-Check' bla machakil
                dependencyCheck additionalArguments: '--project "TP-Jenkins" --scan . --format HTML --out dependency-check-report.html --failOnCVSS 7', odcInstallation: 'DP-Check'
            }
        }
    }
    
    // Had l'étape dima kaddouz f lkher bach t-sauvegardi lik l'rapport
    post {
        always {
            archiveArtifacts artifacts: 'dependency-check-report.html', allowEmptyArchive: true
        }
    }
}
