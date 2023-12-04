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

         /*stage('OWASP Scan') {
            steps {
                dependencyCheck additionalArguments: '--scan ./ ', odcInstallation: 'OWASP Dependency-Check Plugin'*/
                //dependencyCheckPublisher pattern: '**/dependency-check-report.xml'
         //  }
      //  } 
        
        stage('Sonarqube Analysis') {
            steps {
                withSonarQubeEnv('sonar-server') {
                    sh "$SONAR_SCANNER_HOME/bin/sonar-scanner -Dsonar.projectKey=MiniProjet -Dsonar.sources=. -Dsonar.host.url=http://localhost:9000 -Dsonar.login=squ_354617236488464883037a4436fc6ca9226dba02"
                }
            }
        }
        stage('Client Tests') {
	        steps {
		        dir('client') {
			        sh 'npm install'
			        sh 'npm test'
		        }
	        }
        }
    }
}
