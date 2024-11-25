pipeline {
    agent any
    stages {
        stage('Checkout') {
            steps {
                // Checkout code from GitHub
                git 'https://github.com/anas5155/hello-from-pipeline.git'
            }
        }
        stage('Run Python Script') {
            steps {
                // Run the Python script (make sure 'python' is available on the agent)
                sh 'python3 hello.py'
            }
        }
        stage('Build Docker Image') {
            steps {
                script {
                    try {
                        docker.build("${DOCKER_IMAGE_NAME}:${IMAGE_TAG}")
                    } catch (Exception e) {
                        echo "Failed to build docker image: ${e.message}"
                        error "Failed to build docker image"
                    }
                }
            }
        }
        stage('Push to DockerHub') {
            steps {
                script {
                    try {
                        withCredentials([usernamePassword(credentialsId: 'my-docker-hub-credentials-id', usernameVariable: 'DOCKER_USERNAME', passwordVariable: 'DOCKER_PASSWORD')]) {
                            sh """
                                echo "$DOCKER_PASSWORD" | docker login -u "$DOCKER_USERNAME" --password-stdin
                                docker push ${DOCKER_IMAGE_NAME}:${IMAGE_TAG}
                            """
                        }
                    } catch (Exception e) {
                        echo "Failed to push Docker Image to registry: ${e.message}"
                        error "Failed to push Docker Image"
                    }
                }
            }
        }
    }
    post {
        success {
            echo 'Python script executed successfully.'
        }
        failure {
            echo 'Python script execution failed.'
        }
    }
}

