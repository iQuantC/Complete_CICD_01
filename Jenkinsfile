pipeline {
	agent any 
	tools {
		nodejs 'NodeJS'
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
		stage('Lint'){
			steps {
				sh 'npm run format'
				sh 'npm run lint'
			}
		}
	}
}
