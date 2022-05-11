pipeline {
    agent any

    stages {
        stage('Push Container') {
            steps {
                echo "Workspace is $WORKSPACE"
                script{
                    docker.withRegistry('https://index.docker.io/v1','DockerHub'){
                        def image = docker.build('jibranhaseeb/python-flask-app')
                        image.push()
                    }
                }
            }
        }
    }
}
