pipeline {
    agent any

    environment {
        DOCKER_IMAGE = "palashdevops/nodejs:latest"
        CONTAINER_NAME = "latestnode"
    }

    stages {

        stage('Checkout Code') {
            steps {
                checkout scm
            }
        }

        stage('Install Dependencies') {
            steps {
                sh 'npm install'
            }
        }

        stage('Build Docker Image') {
            steps {
                sh "docker build -t $DOCKER_IMAGE ."
            }
        }

        stage('Push Docker Image') {
            steps {
                script {
                    withDockerRegistry([credentialsId: 'docker-new']) {
                        sh "docker push $DOCKER_IMAGE"
                    }
                }
            }
        }

        stage('Deploy Container') {
            steps {
                sh '''
                docker rm -f latestnode || true
                docker run -d -p 8081:8081 --name latestnode palashdevops/nodejs:latest
                '''
            }
        }
    }
}
