pipeline {
    agent any

    environment {
        IMAGE_NAME = "amit4535/demo"
        DOCKER_HOST_IP = "18.212.5.239"
    }

    stages {

        stage('Checkout') {
            steps {
                git 'https://github.com/Amit-4535/docker-cicd-demo.git'
            }
        }

        stage('Build Docker Image') {
            steps {
                sh 'docker build -t $IMAGE_NAME:latest .'
            }
        }

        stage('Push to DockerHub') {
            steps {
                withCredentials([usernamePassword(credentialsId: 'dockerhub', usernameVariable: 'DOCKER_USER', passwordVariable: 'DOCKER_PASS')]) {
                    sh 'echo $DOCKER_PASS | docker login -u $DOCKER_USER --password-stdin'
                    sh 'docker push $IMAGE_NAME:latest'
                }
            }
        }

        stage('Deploy to Docker Host') {
            steps {
                sshagent(credentials: ['docker_jenkins_connection']) {
                    sh """
                        ssh -o StrictHostKeyChecking=no root@$DOCKER_HOST_IP '
                            docker pull $IMAGE_NAME:latest &&
                            docker stop demo || true &&
                            docker rm demo || true &&
                            docker run -d --name demo -p 80:80 $IMAGE_NAME:latest
                        '
                    """
                }
            }
        }
    }
}

