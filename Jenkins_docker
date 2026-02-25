pipeline {
    agent any
    environment {
        IMAGE_NAME = "python-app"
        DOCKERHUB_USER = "syed5zan"
    }

    stages {
        stage('Clone') {
            steps {
                git 'https://github.com/betawins/Python-app.git'
            }
        }

        stage('Build') {
            steps {
                sh 'docker build -t $DOCKERHUB_USER/$IMAGE_NAME:v1 .'
            }
        }

        stage('Push') {
            steps {
                withCredentials([usernamePassword(
                    credentialsId: 'dockerhub-creds',
                    usernameVariable: 'USER',
                    passwordVariable: 'PASS'
                )]) {
                    sh '''
                    echo $PASS | docker login -u $USER --password-stdin
                    docker push $DOCKERHUB_USER/$IMAGE_NAME:v1
                    '''
                }
            }
        }
    }
}
