pipeline {
	agent any
	stages {
		stage('Build') {
			steps {
				echo 'Downloading and building...'
				git branch: 'Grupa02-EK306459_Lab07', url: https://github.com/InzynieriaOprogramowaniaAGH/MIFT2021
				dir('Grupy/Grupa02/EK306459/Lab07/Docker) {
				sh 'sudo curl -L "https://github.com/docker/compose/releases/download/1.29.1/docker-compose-$(uname -s)-$(uname -m)" -o ~/docker-compose'
				sh 'chmod +x ~/docker-compose'
				sh '~/docker-compose -d lab05_chat'
			}
		}
	}

}
