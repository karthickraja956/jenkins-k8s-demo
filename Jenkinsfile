pipeline {
    agent any

    environment {
        IMAGE_NAME = "my-node-app"
        REGISTRY = "docker.io/karthick820"
        DEPLOYMENT_NAME = "node-app"
        NAMESPACE = "default"
    }

    stages {
        stage('Checkout') {
            steps {
                git branch: 'main', url: 'https://github.com/karthickraja956/jenkins-k8s-demo.git'
            }
        }

        stage('Build Docker Image') {
            steps {
                script {
                    docker.build("${IMAGE_NAME}", "app")
                }
            }
        }

        stage('Push to Docker Hub') {
            steps {
                withCredentials([usernamePassword(credentialsId: 'd3c4d27c-c4b3-4309-b5d2-47648db85e79', usernameVariable: 'karthick820', passwordVariable: 'Karthick10')]) {
                    script {
                        docker.withRegistry("https://${REGISTRY}", 'dockerhub-creds') {
                            docker.image("${IMAGE_NAME}").push("latest")
                        }
                    }
                }
            }
        }

        stage('Deploy to Kubernetes') {
            steps {
                script {
                    sh "kubectl set image deployment/${DEPLOYMENT_NAME} ${DEPLOYMENT_NAME}=${REGISTRY}/${IMAGE_NAME}:latest --namespace=${NAMESPACE}"
                }
            }
        }
    }
}
