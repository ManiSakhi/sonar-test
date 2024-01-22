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

        stage("Quality Gate") {
        timeout(time: 1, unit: 'HOURS') {
            waitForQualityGate abortPipeline: true
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
