pipeline {
    agent any

    environment {
        DOCKER_IMAGE = "sudhakshara/pythonflaskapp"
        DOCKER_IMAGE_SQL = "sudhakshara/sqlapp"
        DOCKER_TAG = "latest"
        KUBECONFIG = '/home/sakshara479/.kube/config' 
        DOCKER_CREDENTIALS = 'dockerhub'
    }

    stages {
        stage('Checkout') {
            steps {
                // Checkout the code from GitHub or another source
                git branch: 'sudha-docker', url: 'https://github.com/sakshara-github/Docker-Project.git'
            }
        }

        stage('Build flask Image') {
            steps {
                script {
                    // Build the Docker image
                    sh "docker build -t ${DOCKER_IMAGE}:${DOCKER_TAG} ."
                }
            }
        }
        stage('Build SQL Image') {
            steps {
                script {
                    // Build the Docker image for the SQL application from the 'sql' directory
                    sh "docker build -t ${DOCKER_IMAGE_SQL}:${DOCKER_TAG} ./mysql"
                }
            }
        }
        stage('Push to Dockerhub') {
            steps {
                script {
                    docker.withRegistry('https://index.docker.io/v1/', DOCKER_CREDENTIALS) {
                        sh 'docker push ${DOCKER_IMAGE}:${DOCKER_TAG}'
                        sh 'docker push ${DOCKER_IMAGE_SQL}:${DOCKER_TAG}'
                    }
                }
            }
        }
        stage('Deploy to Kubernetes') {
            steps {
                script {
                        sh "kubectl apply -f frontend.yaml"
                    }
                }
            }
    }
}
