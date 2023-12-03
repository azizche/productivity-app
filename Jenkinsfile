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
    }
}
