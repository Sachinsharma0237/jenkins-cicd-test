pipeline {
    agent any
    environment {
        DOCKERHUB_CREDENTIALS = credentials('2ca01db9-6667-4c8d-bb34-096c5b340318') // Jenkins credentials ID
        DOCKER_IMAGE = "sachinsharma0237/jenkins-cicd-test" // change to your repo
    }
    stages {
        stage('Checkout Code') {
            steps {
                git branch: 'main', url: 'https://github.com/Sachinsharma0237/jenkins-cicd-test.git'   //instead of main write the branch
            }
        }
        stage('Build Docker Image') {
            steps {
                script {
                    bat "docker build -t ${DOCKER_IMAGE}:${BUILD_NUMBER} ."
                }
            }
        }
        stage('Push to DockerHub') {
            steps {
                script {
                    powershell """
                        \$env:DOCKERHUB_CREDENTIALS_PSW | docker login -u \$env:DOCKERHUB_CREDENTIALS_USR --password-stdin
                    """
                    bat "docker push ${DOCKER_IMAGE}:${BUILD_NUMBER}"
                    bat "docker tag ${DOCKER_IMAGE}:${BUILD_NUMBER} ${DOCKER_IMAGE}:latest"
                    bat "docker push ${DOCKER_IMAGE}:latest"
                }
            }
        }
        stage('Deploy Container') {
            steps {
                script {
                    bat "docker stop jenkins-cicd-test 2>nul || docker rm jenkins-cicd-test 2>nul || echo Container cleanup completed"
                    bat "docker run -d --name jenkins-cicd-test -p 3000:3000 ${DOCKER_IMAGE}:latest"
                }
            }
        }
    }
}