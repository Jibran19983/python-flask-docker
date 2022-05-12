pipeline{

	agent any

	environment {
		DOCKERHUB_CREDENTIALS=credentials('DockerHub')
	}

	stages {

		stage('Building Image') {

			steps {
				sh 'docker build -t jibranhaseeb/python-flask-app:latest .'
			}
		}

		stage('Login') {

			steps {
				sh 'echo $DOCKERHUB_CREDENTIALS_PSW | docker login -u $DOCKERHUB_CREDENTIALS_USR --password-stdin'
			}
		}

		stage('Push') {

			steps {
				sh 'docker push jibranhaseeb/python-flask-app:latest'
			}
		}

		stage('Deploy the image in kubernetes cluster') {

			steps {
				sh 'kubectl get pods'
			}
		}
	}

	post {
		always {
			sh 'docker logout'
		}
	}

}