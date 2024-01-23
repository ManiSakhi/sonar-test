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
                        sh "${scannerHome}/bin/sonar-scanner"
                    }
                }
            }
        }

        stage('Quality Gate') {
            steps {
                script {
                    // Wait for the Quality Gate to complete
                    def qg = waitForQualityGate()

                    // Check the status of the Quality Gate
                    if (qg.status == 'ERROR') {
                        error "Quality Gate failed: ${qg.status}"
                    } else if (qg.status == 'OK') {
                        echo "Quality Gate passed: ${qg.status}"
                    } else if (qg.status == 'WARN') {
                        // Handle warning condition, if applicable
                        echo "Quality Gate has a warning: ${qg.status}"
                    } else {
                        // Handle other conditions as needed
                        echo "Quality Gate status: ${qg.status}"
                    }
                }
            }
        }
    }
}
