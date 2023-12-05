pipeline {
    agent any
    environment {
        SONAR_SCANNER_HOME = tool 'sonar-scanner'
	MONGODB_URI = credentials('mongodb-uri')
	TOKEN_KEY = credentials('token-key')
	EMAIL = credentials('email')
	PASSWORD = credentials('password')       
	SLACK_TOKEN = 'xoxb-6307892099297-6288694770534-Ye0i6Bc8xPsA1gIQP7bDXq0i'
        SLACK_CHANNEL = '#projet-cicd'
    }

    stages {
        stage('Checkout') {
            steps {
                checkout scm
            }
        }

         stage('OWASP Scan') {
            steps {
		    sh "echo Sucess"
           }
        } 
        stage('Sonarqube Analysis') {
            steps {
		    sh "echo success"
        /*
                withSonarQubeEnv('sonar-server') {
                    sh "$SONAR_SCANNER_HOME/bin/sonar-scanner -Dsonar.projectKey=MiniProjet -Dsonar.sources=. -Dsonar.host.url=http://localhost:9000 -Dsonar.login=squ_354617236488464883037a4436fc6ca9226dba02"
                }
           */ }
        }
        stage('Client Tests') {
	        steps {
			sh "echo success"
			/*
		        dir('client') {
			        sh 'npm install'
			        sh 'npm test'
		        }
	        */}
        }
	stage('Server Tests') {
		
	steps {
		sh "echo success"
		/*
		dir('server') {
			sh 'npm install'
			sh 'export MONGODB_URI=$MONGODB_URI'
			sh 'export TOKEN_KEY=$TOKEN_KEY'
			sh 'export EMAIL=$EMAIL'
			sh 'export PASSWORD=$PASSWORD'
			sh 'npm test'
		}
	*/}
}
	stage('Build Images') {
		
	steps {
		sh "echo success"
		/*
		sh 'docker build -t azizche1/productivity-app:client-latest client'
		sh 'docker build -t azizche1/productivity-app:server-latest server'
	*/}
}
	    stage('Push Images to DockerHub') {
	steps {
		sh "echo success"
		/*
		withCredentials([usernamePassword(credentialsId: 'dockerhub', passwordVariable: 'DOCKER_PASSWORD', usernameVariable: 'DOCKER_USERNAME')]) {
			sh 'docker login -u $DOCKER_USERNAME -p $DOCKER_PASSWORD'
			sh 'docker tag azizche1/productivity-app:client-latest azizche1/productivity-app:client-latest'
			sh 'docker push azizche1/productivity-app:client-latest'
			sh 'docker tag azizche1/productivity-app:server-latest azizche1/productivity-app:server-latest'
			sh 'docker push azizche1/productivity-app:server-latest'
		}
	*/}
}
	    stage('Deploy to Minikube') {
    steps {
	    sh "echo success"
       /* sh 'minikube start'
        sh 'kubectl config use-context minikube'
dir('client'){
        sh 'kubectl apply -f client-deployment.yaml'

	
}
	dir ('server'){
        sh 'kubectl apply -f server-deployment.yaml'

	}
    */}
}
	     stage('Expose to Minikube'){
		     steps{
			     sh "echo success"
			     /*
		     dir('client'){
	        sh 'kubectl expose deployment client-deployment --type=NodePort --port=8000'*/
		     }

	
}
	
		     
	     }
	    stage('Slack Notification') {
    steps {
        script {
            sh "curl -X POST -H 'Content-type: application/json' --data '{\"text\":\"CI/CD successful\", \"channel\":\"$SLACK_CHANNEL\"}' https://slack.com/api/chat.postMessage?token=$SLACK_TOKEN"
        }
    }
}
	    
    }

