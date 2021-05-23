pipeline {
	agent any
	environment {
		buildSuccess = 'success'
		testSuccess = 'success'
		deploySuccess = 'success'
		
	}
	stages {
		stage('Build') {
			steps {
				echo 'automation test'
				sh 'false'
				echo 'Downloading and building...'
				git branch: 'Grupa02', url: 'https://github.com/InzynieriaOprogramowaniaAGH/MIFT2021'
				dir('Grupy/Grupa02/EK306459/Docker') {
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
						deploySuccess = 'not reached'
					}
				}
			}
		}
		
		
		stage('Test') {
			when { expression { buildSuccess == 'success' } }
			steps {
				dir('Grupy/Grupa02/EK306459/Docker') {
					echo 'Build finished'
					echo 'Testing...'
					sh '~/docker-compose up -d lab05_test' 
					}
				}
			post {
				failure {
					script {
						testSuccess = 'failure'
						deploySuccess = 'not reached'
					}
				}
			}
		}
		stage('Deploy') {
			when { expression { testSuccess == 'success' } }
			steps {
				dir('Grupy/Grupa02/EK306459/Docker') {
					echo 'Testing finished'
					echo 'Deploying latest build...'
					sh '~/docker-compose up -d lab05_deploy'
					sh 'docker tag lab05_deploy:latest perkele4040/perkelerepo'
				}
			}
			post {
				failure {
					script {
						deploySuccess = 'failure'
					}
				}
			}
		}
	}

	post {
		success {
			emailext attachLog: true,
			body: "Build stage: ${buildSuccess}, testing stage: ${testSuccess}, deploy stage: ${deploySuccess}, logs in attachment.",
			subject: 'Build succesfull',
			to: 'emil_kobylecki@onet.eu'
		}

		failure {
			emailext attachLog: true,
			body: "Build stage: ${buildSuccess}, testing stage: ${testSuccess}, deploy stage: ${deploySuccess}, logs in attachment.",
			subject: 'Build unsuccesfull',
			to: 'emil_kobylecki@onet.eu'
		}
	}
}
