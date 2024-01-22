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
                    // Wait for the Quality Gate to complete
                    def qg = waitForQualityGate()

                    // Check the status of the Quality Gate
                    if (qg.status != 'OK') {
                        error "Quality Gate failed: ${qg.status}"
                    }

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
