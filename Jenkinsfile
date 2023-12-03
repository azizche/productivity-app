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
        stage('Sonarqube Analysis') {
            steps {
                withSonarQubeEnv('sonar-server'){
                   sh ''' $SCANNER_HOME/bin/sonar-scanner -Dsonar.projectName=productivity-app
 \
                   -Dsonar.java.binaries=. \
                   -Dsonar.projectKey=productivity-app '''
               }
            }
        }
    }
}
