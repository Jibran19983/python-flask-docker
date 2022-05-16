pipeline{

	agent any

	environment {
		DOCKERHUB_CREDENTIALS=credentials('DockerHub')
		// KUBECONFIG="/etc/rancher/rke2/rke3.yaml"
		TAG = "alpha"
	}

	stages {

		stage('Skip Build') {
                steps {
                    scmSkip(skipPattern:'.*\\[ci skip\\].*')
                }
            }

		stage('Building Image') {

			steps {
				sh "docker build -t jibranhaseeb/python-flask-app:${TAG} ."
			}
		}

		stage('Login to Docker Hub') {

			steps {
				sh 'echo $DOCKERHUB_CREDENTIALS_PSW | docker login -u $DOCKERHUB_CREDENTIALS_USR --password-stdin'
			}
		}

		stage('Push Image') {

			steps {
				sh "docker push jibranhaseeb/python-flask-app:${TAG}"
			}
		}
		stage("changing the tag in the file"){
			steps{
				sh "sed -i 's|newTag: .*|newTag: ${TAG}|' ./cluster/kustomization.yaml"
			}
		}
		stage("pushing to git"){
			steps{
				script{
					withCredentials([string(credentialsId: 'Git', variable: 'SECRET')]) {
						sh ("git add .")
						sh ("git commit -m 'some changes made'")
                        sh("git push https://${SECRET}@github.com/Jibran19983/python-flask-docker.git master")
						
                    }
				}
			}
		}
		// stage('Deploy the image in kubernetes cluster') {
		// 	steps{
		// 		script{
		// 			withKubeConfig([credentialsId: 'Kubernetes', serverUrl: 'https://127.0.0.1:6443']) {
      	// 			sh 'kubectl apply -f ./cluster/flask-app.yml'
    	// 	}

		// 	}
		// 	}
		// }
		// stage('Testing the Web app') {

		// 	steps{
		// 		script{
		// 			withKubeConfig([credentialsId: 'Kubernetes', serverUrl: 'https://127.0.0.1:6443']) {
		// 			sleep 10
		// 			def ip = sh script: 'kubectl get service/my-flask-service -o jsonpath="{.spec.clusterIP}"', returnStdout: true
        //   			ip = ip.trim()
		// 			def result = sh script:"curl -s -o /dev/null -w '%{http_code}' ${ip}:8080", returnStdout: true
		// 			if(result=="200"){
		// 				echo "Pipeline Implemented Successfully"
		// 			}
		// 			else{
		// 				error("Test Failed")
		// 			}
					
    	// 	}

		// 	}
		// 	}
		// }
	}

	post {
		always {
			sh 'docker logout'
		}
		success{
			script{
					withCredentials([string(credentialsId: 'EmailAddress', variable: 'EMAIL')]) {
					emailext body: 'Your pipeline is successfully built',
					subject: 'Pipeline Successful',
					to: EMAIL
			}
			
    }
			
		}
		failure{
			script{
					withCredentials([string(credentialsId: 'EmailAddress', variable: 'EMAIL')]) { 
					emailext body: 'Your pipeline failes',
					subject: 'Pipeline Failed',
					to: EMAIL
			}
			
		}
	}
}
}