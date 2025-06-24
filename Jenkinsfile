pipeline {
    agent any

    environment {
    IMAGE_NAME = "my-node-app"
    REGISTRY = "karthick820"
    //Docker_ID=credentials('Docker_PAT')
    FULL_IMAGE = "docker.io/${REGISTRY}/${IMAGE_NAME}"
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
            docker.build("${FULL_IMAGE}", "app")
        }
    }
}

        stage('Push to Docker Hub') {
            steps {
                withCredentials([usernamePassword(credentialsId: 'd3c4d27c-c4b3-4309-b5d2-47648db85e79', usernameVariable: 'karthick820', passwordVariable: 'Karthick10')]) {
                    script {
                        docker.withRegistry('https://index.docker.io/v1/', "{Docker_ID}"){
                            docker.image("${REGISTRY}/${IMAGE_NAME}").push("latest")
                        }
                    }
                }
            }
        }
        stage('Deploy to Kubernetes') {
            steps {
                script {
                    try {
                        sh 'kubectl apply --validate=false -f k8s/node-app-deployment.yaml'
                        echo "Kubernetes deployment successful."
                    } catch (Exception e) {
                        echo "Kubernetes deployment failed: ${e}"
                        currentBuild.result = 'UNSTABLE'  // mark build unstable instead of failed
                        // Optional: add notifications or other recovery steps here
                    }
                }
            }
        }

       
    }
}
