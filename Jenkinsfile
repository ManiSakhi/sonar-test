pipeline {
    agent any

    stages {
        stage('SCM') {
            steps {
                checkout scm
            }
        }

        stage('SonarQube Analysis') {
            steps {
                script {
                    def scannerHome = tool 'sonarqube'
                    withSonarQubeEnv() {
                        sh "${scannerHome}/bin/sonar-scanner -Dsonar.scanner.metadataFilePath=${WORKSPACE}/sonar-report/report-task.txt"
                    }
                }
            }
        }

       stage('Quality Gate') {
    steps {
        script {
            def qg = waitForQualityGate()
            
            // Check if the Quality Gate status is IN_PROGRESS
            if (qg.status == 'IN_PROGRESS') {
                // The Quality Gate is still in progress, handle accordingly
                error "Quality Gate is still in progress. Please check later."
            } else if (qg.status != 'OK') {
                // The Quality Gate failed
                error "Quality Gate failed: ${qg.status}"
            } else {
                // The Quality Gate passed
                echo "Quality Gate passed: ${qg.status}"
            }
        }
    }
}

    post {
        success {
            echo 'Pipeline succeeded! Ready for deployment.'
        }
        failure {
            echo 'Pipeline failed. Quality Gate check failed.'
        }
    }
}
