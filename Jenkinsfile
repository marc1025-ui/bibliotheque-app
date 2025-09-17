pipeline {
	agent any

	tools {
		maven 'M3'
		jdk 'JDK11'
	}

	stages {
		stage('Checkout') {
			steps {
				git branch: 'main', url: 'https://github.com/votre-utilisateur/bibliotheque-app.git'
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
				withSonarQubeEnv('SonarQube') {
					sh 'mvn sonar:sonar'
				}
			}
		}
	}
}