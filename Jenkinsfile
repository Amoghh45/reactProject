pipeline {
    agent any
    environment {
        DOCKER_HUB_CREDENTIALS = credentials('dockerhub-credentials')  // Ensure the credentials ID is correct
        DOCKER_IMAGE = "amogh69/reactproject"  // Docker image name (ensure it's correct)
    }
    stages {
        stage('Checkout') {
            steps {
                git branch: 'main', url: 'https://github.com/Amoghh45/reactProject.git'
            }
        }
        stage('Build') {
            steps {
                script {
                    docker.build(DOCKER_IMAGE)  // Build the Docker image
                }
            }
        }
        stage('Push to Docker Hub') {
            steps {
                script {
                    docker.withRegistry('https://index.docker.io/v1/', DOCKER_HUB_CREDENTIALS) {
                        docker.image(DOCKER_IMAGE).push("latest")  // Push image with "latest" tag
                        // Optional: You can also tag with the commit hash or version
                        // docker.image(DOCKER_IMAGE).push("${env.GIT_COMMIT}")
                    }
                }
            }
        }
        stage('Deploy to Kubernetes') {
            steps {
                script {
                    // Make sure kubectl is configured and authenticated
                    sh "kubectl apply -f k8s-deployment.yaml"  // Apply Kubernetes deployment
                    sh "kubectl apply -f k8s-service.yaml"  // Apply Kubernetes service
                }
            }
        }
    }
}
