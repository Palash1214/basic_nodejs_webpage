pipeline {
    agent any

    stages {

        stage('Checkout Code') {
            steps {
                git 'https://github.com/Palash1214/basic_nodejs_webpage.git'
            }
        }

        stage('Build Docker Image') {
            steps {
                sh 'docker build -t html-app .'
            }
        }

        stage('Run Container') {
            steps {
                sh 'docker run -d -p 8080:80 html-app'
            }
        }
    }
}
