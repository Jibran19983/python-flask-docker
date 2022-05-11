pipeline {
    agent any

    stages {
        stage('Push Container') {
            steps {
                echo "Workspace is $WORKSPACE"
                script{
                    docker.withRegistry('https://hub.docker.com/','DockerHub'){
                        def image = docker.build('jibranhaseeb/python-flask-app')
                        image.push()
                    }
                }
            }
        }
    }
}
