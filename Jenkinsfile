pipeline {
    agent any

    environment {
        IMAGE_NAME = "amit4535/demo"
    }

    stages {
        stage('Clone Repo') {
            steps {
                git 'https://github.com/Amit-4535/docker-cicd-demo.git'
            }
        }

        stage('Build Docker Image') {
            steps {
                sh 'docker build -t $IMAGE_NAME:latest .'
            }
        }

        stage('Login to DockerHub') {
            steps {
                withCredentials([usernamePassword(credentialsId: 'dockerhub', usernameVariable: 'DOCKER_USER', passwordVariable: 'DOCKER_PASS')]) {
                    sh 'echo $DOCKER_PASS | docker login -u $DOCKER_USER --password-stdin'
                }
            }
        }

        stage('Push Docker Image') {
            steps {
                sh 'docker push $IMAGE_NAME:latest'
            }
        }

        stage('Clean Up') {
            steps {
                sh 'docker rmi $IMAGE_NAME:latest || true'
            }
        }
    }
}

