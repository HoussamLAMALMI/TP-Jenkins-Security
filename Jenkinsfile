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
        
        stage('SAST Scan') {
            steps {
                catchError(buildResult: 'SUCCESS', stageResult: 'FAILURE') {
                    script {
                        def sonarScanner = tool name: 'SonarScanner'
                        sh "${sonarScanner}/bin/sonar-scanner -Dsonar.projectKey=TP-Jenkins -Dsonar.sources=."
                    }
                }
            }
        }
        
        stage('SCA Scan') {
            steps {
                script {
                    // Récupération sécurisée du chemin de l'outil
                    def dpCheck = tool name: 'DP-Check'
                    
                    // On lance le scan et on spécifie le fichier de sortie (--out)
                    sh "${dpCheck}/bin/dependency-check.sh --project 'TP-Jenkins' --scan . --format HTML --out dependency-check-report.html --failOnCVSS 7"
                }
            }
        }
    }
    
    // Cette section s'exécute à la fin du pipeline
    post {
        always {
            // Sauvegarde le rapport généré pour le rendre téléchargeable/visible sur Jenkins
            archiveArtifacts artifacts: 'dependency-check-report.html', allowEmptyArchive: true
        }
    }
}
