pipeline {
	agent any
	
	tools {
		maven 'maven-3.6.3'
	}
	
	stages {
		stage('Build') {
			steps {
				bat 'mvn clean package'
			}
			post {
				success {
					echo 'Now Archiving...'
					archiveArtifacts artifacts: '**/target/*.war'
				}
			}
		}
		
		stage('Deploy to Staging') {
			steps {
				build job: 'deploy-to-staging2'
			}
		}
		
		stage('Deploy to Production') {
			steps {
				timeout(time:5, unit:'DAYS') {
					input message:'Approve PRODUCTION Deployment?'
				}
				
				build job: 'deploy-to-prod2'
			}
			
			post {
				success {
					echo 'Code deployed to Production.'
				}
				
				failure {
					echo 'Deployment failed.'
				}
			}
		}
	}
}