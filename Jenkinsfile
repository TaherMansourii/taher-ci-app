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
                script {
                    sh "git clone -b main https://github.com/TaherMansourii/taher-ci-app.git . || true"
                    sh "git fetch origin || true"
                    sh "git reset --hard origin/main || true"
                }
            }
        }

        stage('Build Docker Image') {
            steps {
                sh "docker build -t $IMAGE:${BUILD_NUMBER} . || true"
            }
        }

        stage('Push to DockerHub') {
            steps {
                sh "echo $DOCKERHUB_PSW | docker login -u $DOCKERHUB_USR --password-stdin || true"
                sh "docker push $IMAGE:${BUILD_NUMBER} || true"
            }
        }

        stage('Deploy to Kubernetes') {
            steps {
                script {
                    // Apply deployment.yaml (creates or updates deployment)
                    sh "kubectl apply -f k8s/deployment.yaml || true"
                    // Set image safely
                    sh "kubectl set image deployment/nginx-deployment nginx=$IMAGE:${BUILD_NUMBER} --record || true"
                }
            }
        }
    }
}
