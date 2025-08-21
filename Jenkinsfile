pipeline {
    agent any

    environment {
        DOCKERHUB_CREDENTIALS = credentials('dockerhub-cred-id') // Your Jenkins credential ID
        DOCKERHUB_REPO = "sachinsharma0237/testapp" // Replace with your DockerHub repo
        APP_NAME = "testapp"
    }

    stages {
        stage('Checkout') {
            steps {
                git branch: 'main', url: 'https://github.com/your-username/your-repo.git'
            }
        }

        stage('Build Docker Image') {
            steps {
                script {
                    bat "docker build -t %DOCKERHUB_REPO%:latest ."
                }
            }
        }

        stage('Push Docker Image') {
            steps {
                script {
                    withCredentials([usernamePassword(credentialsId: 'dockerhub-cred-id', usernameVariable: 'DOCKER_USER', passwordVariable: 'DOCKER_PASS')]) {
                        bat "docker login -u %DOCKER_USER% -p %DOCKER_PASS%"
                        bat "docker push %DOCKERHUB_REPO%:latest"
                    }
                }
            }
        }

        stage('Deploy Container') {
            steps {
                script {
                    // Stop old container if running
                    bat "docker stop %APP_NAME% || echo No container to stop"
                    bat "docker rm %APP_NAME% || echo No container to remove"

                    // Run new container
                    bat "docker run -d --name %APP_NAME% -p 8080:8080 %DOCKERHUB_REPO%:latest"
                }
            }
        }
    }

    post {
        always {
            echo 'Pipeline finished!'
        }
    }
}
