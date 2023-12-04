pipeline {
    agent any
    environment {
        SONAR_SCANNER_HOME = tool 'sonar-scanner'
	MONGODB_URI = credentials('mongodb-uri')
	TOKEN_KEY = credentials('token-key')
	EMAIL = credentials('email')
	PASSWORD = credentials('password')
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
	stage('Server Tests') {
	steps {
		dir('server') {
			sh 'npm install'
			sh 'export MONGODB_URI=$MONGODB_URI'
			sh 'export TOKEN_KEY=$TOKEN_KEY'
			sh 'export EMAIL=$EMAIL'
			sh 'export PASSWORD=$PASSWORD'
			sh 'npm test'
		}
	}
}
	stage('Build Images') {
	steps {
		sh 'docker build -t azizche1/productivity-app:client-latest client'
		sh 'docker build -t azizche1/productivity-app:server-latest server'
	}
}
	    stage('Push Images to DockerHub') {
	steps {
		withCredentials([usernamePassword(credentialsId: 'dockerhub', passwordVariable: 'DOCKER_PASSWORD', usernameVariable: 'DOCKER_USERNAME')]) {
			sh 'docker login -u $DOCKER_USERNAME -p $DOCKER_PASSWORD'
			sh 'docker tag azizche1/productivity-app:client-latest azizche1/productivity-app:client-latest'
			sh 'docker push azizche1/productivity-app:client-latest'
			sh 'docker tag azizche1/productivity-app:server-latest azizche1/productivity-app:server-latest'
			sh 'docker push azizche1/productivity-app:server-latest'
		}
	}
}
	    stage('Deploy to Minikube') {
    steps {
        sh 'minikube start'
        sh 'kubectl config use-context minikube'
dir('client'){
        sh 'kubectl apply -f client-deployment.yaml'
	        sh 'kubectl expose deployment client-deployment --type=NodePort --port=80'

	
}
	dir ('server'){
        sh 'kubectl apply -f server-deployment.yaml'
	 sh 'kubectl expose deployment server-deployment --type=NodePort --port=80'

	}
    }
}
    }
}
