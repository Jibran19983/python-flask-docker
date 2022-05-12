pipeline{

	agent any

	environment {
		DOCKERHUB_CREDENTIALS=credentials('DockerHub')
		KUBECONFIG="/etc/rancher/rke2/rke3.yaml"
	}

	stages {

		// stage('Building Image') {

		// 	steps {
		// 		sh 'docker build -t jibranhaseeb/python-flask-app:latest .'
		// 	}
		// }

		// stage('Login') {

		// 	steps {
		// 		sh 'echo $DOCKERHUB_CREDENTIALS_PSW | docker login -u $DOCKERHUB_CREDENTIALS_USR --password-stdin'
		// 	}
		// }

		// stage('Push') {

		// 	steps {
		// 		sh 'docker push jibranhaseeb/python-flask-app:latest'
		// 	}
		// }

		stage('Deploy the image in kubernetes cluster') {

			steps {
				sh 'kubectl get pods'
			}
		}
		stage('test cluster') {

			steps {
				script{
					def j = sh (script:'curl -s -o /dev/null -w "%{http_code}\n" http://www.google.com/',returnStdout:true)
					// println j
				}
				

			}
		}
	}

	post {
		always {
			sh 'docker logout'
		}
	}

}