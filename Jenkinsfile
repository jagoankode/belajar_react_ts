pipeline {
    agent any

    stages {
        stage('Checkout') {
            steps {
                git branch: 'main', url: 'https://github.com/jagoankode/belajar_react_ts.git'
            }
        }

        stage('Build Docker Image') {
            steps {
                sh 'docker build -t react-app .'
            }
        }

        stage('Run Docker on Port 3000') {
            steps {
                sh '''
                docker rm -f react-app || true
                docker run -d -p 3000:3000 --name react-app react-app
                '''
            }
        }
    }
}