pipeline {
    agent any

    environment {
        DOCKERHUB = credentials('dockerhub')       
        IMAGE = "tahermansourii/taher-ci-app"
        KUBECONFIG = '/var/lib/jenkins/.kube/config'
    }

    stages {
        stage('Checkout') {
            steps {
                git branch: 'main', url: 'https://github.com/TaherMansourii/taher-ci-app.git'
            }
        }

        stage('Build Docker Image') {
            steps {
                sh "docker build -t $IMAGE:${BUILD_NUMBER} ."
            }
        }

        stage('Push to DockerHub') {
            steps {
                sh "echo $DOCKERHUB_PSW | docker login -u $DOCKERHUB_USR --password-stdin"
                sh "docker push $IMAGE:${BUILD_NUMBER}"
            }
        }

        stage('Deploy to Kubernetes') {
            steps {
                sh "kubectl apply -f k8s/deployment.yaml"
                sh "kubectl set image deployment/nginx-deployment nginx=$IMAGE:${BUILD_NUMBER}"
            }
        }
    }
}
