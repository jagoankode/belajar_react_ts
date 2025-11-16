pipeline {
    agent any

    stages {
        stage("Checkout") {
            steps {
                git branch: 'main', url: 'https://github.com/jagoankode/belajar_react_ts.git'
            }
        }

        stage("Build") {
            steps {
                echo "Build step running..."
            }
        }

        stage("Test") {
            steps {
                echo "Running tests..."
            }
        }
    }
}
