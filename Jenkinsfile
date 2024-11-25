pipeline {
    agent any
	environment {
        
        	DOCKER_IMAGE_NAME = 'anas5155/python-hello-docker-jenkins'
        	IMAGE_TAG = '1.0'
	}
    stages {
        stage('Checkout') {
            steps {
                // Ensure you're checking out the correct branch
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
                        docker.build("python-hello-docker-jenkins")
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
                        withCredentials([usernamePassword(credentialsId: 'my-docker-hub-credentials-id', usernameVariable: 'DOCKER_USERNAME', passwordVariable: 'DOCKER_PASSWORD')]) {
                            sh """
                                echo "$DOCKER_PASSWORD" | docker login -u "$DOCKER_USERNAME" --password-stdin
                                docker push python-hello-cdac
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

