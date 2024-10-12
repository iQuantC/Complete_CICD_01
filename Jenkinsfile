pipeline {
	agent any 
	tools {
		nodejs 'NodeJS'
	}	
	environment {
		SONAR_SCANNER_HOME = tool 'SonarQubeScanner'
		SONAR_PROJECT_KEY = 'complete-cicd-01'
		DOCKER_HUB_REPO = 'iquantc/complete-cicd-01'
		DOCKER_HUB_CRED_ID = 'cicd-01-dhub-token'
	}
	stages {
		stage('GitHub'){
			steps {
				git branch: 'main', credentialsId: 'jen-git-min', url: 'https://github.com/iQuantC/Complete_CICD_01.git'
			}
		}
		stage('Unit Tests'){
			steps {
				sh 'npm test'
				sh 'npm install'
			}

		}
		stage('SonarQube Analysis'){
			steps {
				withCredentials([string(credentialsId: 'sonar-token', variable: 'SONAR_TOKEN')]) {
    					
					withSonarQubeEnv('SonarQube') {
						sh """
						${SONAR_SCANNER_HOME}/bin/sonar-scanner \
						-Dsonar.projectKey=${SONAR_PROJECT_KEY} \
						-Dsonar.sources=. \
						-Dsonar.host.url=http://sonarqube:9000 \
						-Dsonar.login=${SONAR_TOKEN}
						"""   						
					}
				}
			}
		}
		stage('Docker Image'){
			steps {
				script {
					dockerImage = docker.build("${DOCKER_HUB_REPO}:latest")
				}
			}
		}
		stage('Trivy Scan'){
			steps {
				sh 'trivy --severity HIGH,CRITICAL --no-progress --format table -o trivy-cicd-01-report.html image ${DOCKER_HUB_REPO}:latest'	
			}
		}
		stage('Push Image to DockerHub'){
			steps {
				script {
					docker.withRegistry('https://registry.hub.docker.com', "${DOCKER_HUB_CRED_ID}"){
						dockerImage.push("latest")
					}
				}	
			}
		}
	}
}
