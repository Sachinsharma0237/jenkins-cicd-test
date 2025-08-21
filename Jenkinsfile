pipeline {
    agent any

    environment {
        DOCKERHUB_CREDENTIALS = credentials('2ca01db9-6667-4c8d-bb34-096c5b340318') // Jenkins credentials ID
        DOCKER_IMAGE = "sachinsharma0237/jenkins-cicd-test" // change to your repo
    }

    stages {
        stage('Checkout Code') {
            steps {
                git branch: 'main', url: 'https://github.com/Sachinsharma0237/jenkins-cicd-test.git'   //instead of main write the branch name
            }
        }

        stage('Build Docker Image') {
            steps {
                script {
                    sh "docker build -t ${DOCKER_IMAGE}:${BUILD_NUMBER} ."
                }
            }
        }

        stage('Push to DockerHub') {
            steps {
                script {
                    sh "echo $DOCKERHUB_CREDENTIALS_PSW | docker login -u $DOCKERHUB_CREDENTIALS_USR --password-stdin"
                    sh "docker push ${DOCKER_IMAGE}:${BUILD_NUMBER}"
                    sh "docker tag ${DOCKER_IMAGE}:${BUILD_NUMBER} ${DOCKER_IMAGE}:latest"
                    sh "docker push ${DOCKER_IMAGE}:latest"
                }
            }
        }

        stage('Deploy Container') {
            steps {
                script {
                    sh "docker stop jenkins-cicd-test || true && docker rm jenkins-cicd-test || true"
                    sh "docker run -d --name jenkins-cicd-test -p 3000:3000 ${DOCKER_IMAGE}:latest"
                }
            }
        }
    }
}
