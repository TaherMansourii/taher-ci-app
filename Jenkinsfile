pipeline {
    agent any

    environment {
        DOCKERHUB = credentials('dockerhub')       // DockerHub credentials ID in Jenkins
        IMAGE = "tahermansourii/taher-ci-app"
    }

    stages {
        stage('Checkout') {
            steps {
                git 'https://github.com/TaherMansourii/taher-ci-app.git'
            }
        }

        stage('Build Docker Image') {
            steps {
                sh "docker build -t $IMAGE:${BUILD_NUMBER} ."
            }
        }

        stage('Push to DockerHub') {
            steps {
                sh "docker login -u $DOCKERHUB_USR -p $DOCKERHUB_PSW"
                sh "docker push $IMAGE:${BUILD_NUMBER}"
            }
        }

        stage('Deploy to Kubernetes') {
            steps {
                sh "kubectl set image deployment/nginx-deployment nginx=$IMAGE:${BUILD_NUMBER}"
            }
        }
    }
}
