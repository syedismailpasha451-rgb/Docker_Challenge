pipeline {
    agent any

    environment {
        DOCKER_IMAGE = "syed5zan/python-app"
        BUILD_TAG = "${env.BUILD_NUMBER}"
    }

    stages {

        stage('Clone Repository') {
            steps {
                git branch: 'main',
                    url: 'https://github.com/betawins/Python-app.git'
            }
        }

        stage('Build Docker Image') {
            steps {
                sh """
                    docker build -t $DOCKER_IMAGE:latest .
                    docker tag $DOCKER_IMAGE:latest $DOCKER_IMAGE:$BUILD_TAG
                """
            }
        }

        stage('Login to DockerHub') {
            steps {
                withCredentials([usernamePassword(
                    credentialsId: 'dockerhub-creds',
                    usernameVariable: 'DOCKER_USER',
                    passwordVariable: 'DOCKER_PASS'
                )]) {
                    sh 'echo $DOCKER_PASS | docker login -u $DOCKER_USER --password-stdin'
                }
            }
        }

        stage('Push Docker Image') {
            steps {
                sh """
                    docker push $DOCKER_IMAGE:latest
                    docker push $DOCKER_IMAGE:$BUILD_TAG
                """
            }
        }

        stage('Cleanup') {
            steps {
                sh 'docker system prune -f'
            }
        }
    }
}
