pipeline {
    agent any

    stages {

        stage('Clone Code') {
            steps {
                git branch: 'main', url: 'https://github.com/YOUR_USERNAME/YOUR_REPO.git'
            }
        }

        stage('Build Docker Image') {
            steps {
                sh 'docker build -t two-tier-flask-app .'
            }
        }

        stage('Push To DockerHub') {
            steps {
                withCredentials([
                    usernamePassword(
                        credentialsId: 'dockerhub-creds',
                        usernameVariable: 'DOCKER_USER',
                        passwordVariable: 'DOCKER_PASS'
                    )
                ]) {

                    sh '''
                    echo $DOCKER_PASS | docker login -u $DOCKER_USER --password-stdin

                    docker tag two-tier-flask-app:latest DOCKER_USERNAME/two-tier-flask-app:latest

                    docker push DOCKER_USERNAME/two-tier-flask-app:latest
                    '''
                }
            }
        }
    }
}
