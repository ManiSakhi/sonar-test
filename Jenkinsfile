pipeline {
    agent any

    stages {
        stage('Build') {
            steps {
                script {
                    // Maven build
                    sh 'mvn clean install'

                    // Run SonarQube analysis
                    withSonarQubeEnv('sonarqube') {
                        sh 'mvn sonar:sonar'
                    }
                }
            }
        }
    }

    post {
        always {
            // Clean up
            deleteDir()
        }
    }
}
