pipeline {
	agent any
	parameters {
		booleanParam(name: 'var', defaultValue: true, description: 'Decides whether to test')
	}
	stages {
		stage('Build') {
			steps {
				echo 'Downloading and building...'
				echo params.var
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
						
						echo var
					}
				}
			}
		}
		
		
		stage('Test') {
			steps {
				script {
					if(var) {
						dir('Grupy/Grupa02/EK306459/Lab07/Docker') {
							echo 'Build finished'
							echo 'Testing...'
							sh '~/docker-compose up -d lab05_test' 
						}
					}
				}
			}
		}
	}

	post {
		success {
			emailext attachLog: true,
			body: 'Build ${env.JOB_NAME} succesfull, logs in attachment.',
			subject: 'Build succesfull',
			to: 'emil_kobylecki@onet.eu'
		}

		failure {
			emailext attachLog: true,
			body: 'Build ${env.JOB_NAME} unsuccesfull, logs in attachment.',
			subject: 'Build unsuccesfull',
			to: 'emil_kobylecki@onet.eu'
		}
	}
}
