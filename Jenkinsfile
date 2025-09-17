pipeline {
	agent any

	tools {
		maven 'M3'
		jdk 'JDK11'
	}

	stages {
		stage('Checkout') {
			steps {
				git branch: 'main',
				url: 'https://github.com/marc1025-ui/bibliotheque-app.git',
				credentialsId: 'github-pat'
			}
		}

		stage('Build') {
			steps {
				sh 'mvn compile'
			}
		}

		stage('Test') {
			steps {
				sh 'mvn test'
			}
			post {
				always {
					junit 'target/surefire-reports/**/*.xml'
				}
			}
		}

		stage('SonarQube Analysis') {
			steps {
				// Récupère le token SonarQube stocké dans Jenkins Credentials
				withCredentials([string(credentialsId: 'SONAR_TOKEN', variable: 'SONAR_TOKEN')]) {
					withSonarQubeEnv('SonarQube') {  // Nom du serveur SonarQube configuré dans Jenkins
						sh 'mvn sonar:sonar -Dsonar.login=$SONAR_TOKEN'
					}
				}
			}
		}
	}
}