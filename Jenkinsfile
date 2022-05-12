pipeline{

	agent any

	environment {
		DOCKERHUB_CREDENTIALS=credentials('DockerHub')
		// KUBECONFIG="/etc/rancher/rke2/rke3.yaml"
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
			steps{
				script{
					withKubeConfig([credentialsId: 'Kubernetes', serverUrl: 'https://127.0.0.1:6443']) {
      				sh 'kubectl delete -f ./cluster/flask-app.yml'
					sh 'kubectl create -f ./cluster/flask-app.yml'
					sleep 30
					def ip = sh script: 'kubectl get service/my-flask-service -o jsonpath="{.spec.clusterIP}"', returnStdout: true
          			ip = ip.trim()
					println ip
    		}

			}
			}
		}
		stage('test cluster') {

			steps{
				script{
					withKubeConfig([credentialsId: 'Kubernetes', serverUrl: 'https://127.0.0.1:6443']) {
					sleep 10
					def ip = sh script: 'kubectl get service/my-flask-service -o jsonpath="{.spec.clusterIP}"', returnStdout: true
          			ip = ip.trim()
					def result = sh script:'curl -s -o /dev/null -w "%{http_code}" ${ip}:8080'
					if(result==200){
						echo "Pipeline Implemented Successfully"
					}
					else{
						error("Test Failed")
					}
					
    		}

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