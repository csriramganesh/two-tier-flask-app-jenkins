pipeline {
    agent any

    stages {

        stage('Clone Code') {
            steps {
                git url: 'https://github.com/csriramganesh/two-tier-flask-app-jenkins' , branch: 'main'
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

                    docker tag two-tier-flask-app:latest DOCKER_USER/two-tier-flask-app:latest

                    docker push DOCKER_USER/two-tier-flask-app:latest
                    '''
                }
            }
        }
    }
}
