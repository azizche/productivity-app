pipeline {
    agent any

    stages {
        stage('Checkout') {
            steps {
                checkout scm
            }
        }

        stage('OWASP Scan') {
            steps {
                dependencyCheck additionalArguments: '--scan ./ ', odcInstallation: 'OWASP Dependency-Check Plugin'
                dependencyCheckPublisher pattern: '**/dependency-check-report.xml'
            }
        }
        stage('SonarQube Analysis') {
            steps {
                script {
                    withSonarQubeEnv('Your_SonarQube_Server_Name') {
                        // Run SonarScanner for Node.js
                        sh "${SONAR_SCANNER_HOME}/bin/sonar-scanner"
                    }
                }
            }
        }
    }
}
