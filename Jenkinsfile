pipeline {
    agent any
    environment {
        DOCKER_IMAGE_NAME = 'anas5155/python-hello-docker-jenkins'
        IMAGE_TAG = '1.0'
    }
    stages {
        stage('Checkout') {
            steps {
                git branch: 'main', url: 'https://github.com/anas5155/hello-from-pipeline.git'
            }
        }
        stage('Run Python Script') {
            steps {
                sh 'python3 hello.py'
            }
        }
        stage('Build Docker Image') {
            steps {
                script {
                    try {
                        docker.build("${DOCKER_IMAGE_NAME}:${IMAGE_TAG}")
                    } catch (Exception e) {
                        echo "Failed to build Docker image: ${e.message}"
                        error "Failed to build Docker image"
                    }
                }
            }
        }
        stage('Push to DockerHub') {
            steps {
                script {
                    try {
                        // Verify the image exists
                        sh "docker images ${DOCKER_IMAGE_NAME}:${IMAGE_TAG}"
                        
                        withCredentials([usernamePassword(credentialsId: 'my-docker-hub-credentials-id', usernameVariable: 'DOCKER_USERNAME', passwordVariable: 'DOCKER_PASSWORD')]) {
                            echo "Docker Image to Push: ${DOCKER_IMAGE_NAME}:${IMAGE_TAG}"
                            sh """
                                echo "$DOCKER_PASSWORD" | docker login -u "$DOCKER_USERNAME" --password-stdin
                                docker push "${DOCKER_IMAGE_NAME}:${IMAGE_TAG}"
                            """
                        }
                    } catch (Exception e) {
                        echo "Failed to push Docker Image: ${e.message}"
                        error "Failed to push Docker Image"
                    }
                }
            }
        }
    }
    post {
        success {
            echo 'Pipeline completed successfully!'
        }
        failure {
            echo 'Pipeline failed.'
        }
    }
}

