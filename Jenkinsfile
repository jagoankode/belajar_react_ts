pipeline {
    agent any

    environment {
        REGISTRY = "docker.io"
        IMAGE_NAME = "jagoankode/belajar-react"   // Ganti dengan nama repo kamu
    }

    stages {
        stage('Checkout') {
            steps {
                git branch: 'main', url: 'https://github.com/jagoankode/belajar_react_ts.git'
            }
        }

        stage('Install Dependencies') {
            steps {
                sh 'npm install'
            }
        }

        stage('Run Tests') {
            steps {
                sh 'npm test -- --watchAll=false || true' 
            }
        }

        stage('Build React App') {
            steps {
                sh 'npm run build'
            }
        }

        stage('Preview Build (localhost test)') {
            steps {
                sh 'npx serve -s build -l 3000 &'
                sh 'sleep 5'
                sh 'curl -I http://localhost:3000'
            }
        }

        stage('Docker Build') {
            steps {
                script {
                    dockerImage = docker.build("${IMAGE_NAME}:${BUILD_NUMBER}")
                }
            }
        }

        stage('Docker Login') {
            steps {
                withCredentials([usernamePassword(credentialsId: 'dockerhub-cred', usernameVariable: 'USER', passwordVariable: 'PASS')]) {
                    sh "echo $PASS | docker login -u $USER --password-stdin"
                }
            }
        }

        stage('Docker Push') {
            steps {
                script {
                    dockerImage.push()
                    dockerImage.push("latest")
                }
            }
        }

        stage('Deploy to Server (SSH)') {
            when {
                branch 'main'
            }
            steps {
                sshagent(['server-ssh']) {
                    sh '''
                    ssh -o StrictHostKeyChecking=no user@your-server-ip "
                        docker pull ${IMAGE_NAME}:latest &&
                        docker stop react-app || true &&
                        docker rm react-app || true &&
                        docker run -d -p 80:80 --name react-app ${IMAGE_NAME}:latest
                    "
                    '''
                }
            }
        }
    }
}