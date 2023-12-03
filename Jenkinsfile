pipeline {
    agent any
    environment {
        SONAR_SCANNER_HOME = tool 'sonar-scanner'
       
    }

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
                    withSonarQubeEnv('sonar-server') {
                        // Run SonarScanner for Node.js
                        sh "${SONAR_SCANNER_HOME}/bin/sonar-scanner"
                    }
                }
            }
        }
    }
}
