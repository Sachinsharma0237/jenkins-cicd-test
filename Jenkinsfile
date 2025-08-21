pipeline {
    agent any

    environment {
        // Replace with your DockerHub username
        DOCKERHUB_USER = 'your-dockerhub-username'
        IMAGE_NAME = 'sample-app'
    }

    stages {
        stage('Checkout') {
            steps {
                git branch: 'main', url: 'https://github.com/your-username/your-sample-repo.git'
            }
        }

        stage('Build Docker Image') {
            steps {
                script {
                    bat "docker build -t %DOCKERHUB_USER%/%IMAGE_NAME%:latest ."
                }
            }
        }

        stage('Login to DockerHub') {
            steps {
                script {
                    withCredentials([usernamePassword(credentialsId: 'dockerhub-cred-id', 
                                                     usernameVariable: 'DOCKER_USER', 
                                                     passwordVariable: 'DOCKER_PASS')]) {
                        bat "docker login -u %DOCKER_USER% -p %DOCKER_PASS%"
                    }
                }
            }
        }

        stage('Push Docker Image') {
            steps {
                script {
                    bat "docker push %DOCKERHUB_USER%/%IMAGE_NAME%:latest"
                }
            }
        }

        stage('Run Container (Optional)') {
            steps {
                script {
                    // Stop old container if running
                    bat '''
                    docker ps -q --filter "name=sample-app" | findstr . && docker stop sample-app && docker rm sample-app || echo "No old container"
                    '''
                    // Run new container
                    bat "docker run -d --name sample-app -p 8080:8080 %DOCKERHUB_USER%/%IMAGE_NAME%:latest"
                }
            }
        }
    }
}
