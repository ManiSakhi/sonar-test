node {
  stage('SCM') {
    checkout scm
  }
  stage('SonarQube Analysis') {
    def scannerHome = tool 'sonarqube';
    withSonarQubeEnv() {
      sh "${scannerHome}/bin/sonar-scanner"
  stage('Quality Gate') {
            steps {
                script {
                    // Check SonarQube quality gate status
                    withSonarQubeEnv('Your SonarQube Server') {
                        def qg = waitForQualityGate()
                        if (qg.status != 'OK') {
                            error "Quality Gate failed: ${qg.status}"
                        }
                    }
                }
            }
        }

        // Add more stages for deployment, testing, etc.
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
       }
    }
  }
