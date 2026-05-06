pipeline {
    agent any

    tools {
        nodejs 'nodejs'   // Configure in Global Tool Configuration
    }

    environment {
        DOCKER_IMAGE = "palashdevops/nodejs:latest"
        CONTAINER_NAME = "latestnode"
    }

    stages {

        stage('Checkout Code') {
            steps {
                // Uses Jenkins default checkout (no duplicate)
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
                script {
                    sh """
                    docker rm -f $CONTAINER_NAME || true
                    docker run -d -p 8081:8081 --name $CONTAINER_NAME $DOCKER_IMAGE
                    """
                }
            }
        }
    }

    post {
        success {
            echo 'Pipeline executed successfully ✅'
        }
        failure {
            echo 'Pipeline failed ❌'
        }
    }
}
