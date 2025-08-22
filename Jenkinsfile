pipeline {
    agent any
    environment {
        DOCKER_IMAGE = "sachinsharma0237/jenkins-cicd-test" // change to your repo
    }
    stages {
        stage('Checkout Code') {
            steps {
                git branch: 'main', url: 'https://github.com/Sachinsharma0237/jenkins-cicd-test.git'
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
                    // ⚠️ For testing only — plain password (NOT safe for production)
                    bat '''
                        docker login -u sachinsharma0237 -p Sachin12345
                        docker push sachinsharma0237/jenkins-cicd-test:%BUILD_NUMBER%
                        docker tag sachinsharma0237/jenkins-cicd-test:%BUILD_NUMBER% sachinsharma0237/jenkins-cicd-test:latest
                        docker push sachinsharma0237/jenkins-cicd-test:latest
                    '''
                }
            }
        }

        stage('Deploy Container') {
            steps {
                script {
                    bat '''
                        docker stop jenkins-cicd-test 2>nul || echo No container to stop
                        docker rm jenkins-cicd-test 2>nul || echo No container to remove
                        docker run -d --name jenkins-cicd-test -p 3000:3000 sachinsharma0237/jenkins-cicd-test:latest
                    '''
                }
            }
        }

        stage('Deploy Locally') {
            steps {
                script {
                    bat """
                    docker stop my-sample-app || exit 0
                    docker rm my-sample-app || exit 0
                    docker pull %DOCKER_IMAGE%
                    docker run -d -p 8080:80 --name my-sample-app %DOCKER_IMAGE%
                    """
                }
            }
        }
    }
}
