pipeline {
    agent any
    environment {
        DOCKER_USER = 'solomon922'
        DOCKER_IMAGE = 'devops-zero-hero'
        DOCKER_TAG = 'latest'
    }
    stages {
        stage('Checkout') {
            steps {
                checkout scm
            }
        }
        stage('Build Docker') {
            steps {
                sh 'docker build -t $DOCKER_IMAGE:$DOCKER_TAG .'
            }
        }
        stage('Push to Docker Hub') {
            steps {
                sh '''
                docker tag $DOCKER_IMAGE:$DOCKER_TAG $DOCKER_USER/$DOCKER_IMAGE:$DOCKER_TAG
                echo $DOCKER_PASSWORD | docker login -u $DOCKER_USER --password-stdin
                docker push $DOCKER_USER/$DOCKER_IMAGE:$DOCKER_TAG
                '''
            }
        }
        stage('Deploy to EC2') {
            steps {
                sh '''
                docker pull $DOCKER_USER/$DOCKER_IMAGE:$DOCKER_TAG
                docker stop devops-zero-hero || true
                docker rm devops-zero-hero || true
                docker run -d --name devops-zero-hero $DOCKER_USER/$DOCKER_IMAGE:$DOCKER_TAG
                '''
            }
        }
    }
}
