pipeline {
	agent any
	environment {
		buildSuccess = 'success'
		testSuccess = 'success'
		
	}
	stages {
		stage('Build') {
			steps {
				echo 'Downloading and building...'
				echo buildSuccess
				git branch: 'Grupa02-EK306459_Lab07', url: 'https://github.com/InzynieriaOprogramowaniaAGH/MIFT2021'
				dir('Grupy/Grupa02/EK306459/Lab07/Docker') {
				sh 'curl -L "https://github.com/docker/compose/releases/download/1.29.1/docker-compose-$(uname -s)-$(uname -m)" -o ~/docker-compose'
				sh 'chmod +x ~/docker-compose'
				sh '~/docker-compose up -d lab05_chat'
				
				}
			}
			post {
				failure {
					script {
						echo 'Build failed'
						
						buildSuccess = 'failure'
						testSuccess = 'not reached'
						echo buildSuccess
					}
				}
			}
		}
		
		
		stage('Test') {
			when { expression { buildSuccess == 'success' } }
			steps {
				dir('Grupy/Grupa02/EK306459/Lab07/Docker') {
					echo 'Build finished'
					echo 'Testing...'
					sh '~/docker-compose up -d lab05_test' 
					}
				}
			post {
				failure {
					script {
						testSuccess = 'failure'
					}
				}
			}
		}
	}

	post {
		success {
			emailext attachLog: true,
				body: 'Build stage: ${buildSuccess}, testing stage: ${testSuccess}, logs in attachment.',
			subject: 'Build succesfull',
			to: 'emil_kobylecki@onet.eu'
		}

		failure {
			emailext attachLog: true,
			body: 'Build stage: ${buildSuccess}, testing stage: ${testSuccess}, logs in attachment.',
			subject: 'Build unsuccesfull',
			to: 'emil_kobylecki@onet.eu'
		}
	}
}
