pipeline {
    agent { label 'linux-node' }

    stages {
        stage('Checkout') {
            steps {
                checkout scm
            }
        }

        stage('Build Docker Image') {
            steps {
                script {
                    sh "docker build -t my-node-app ."
                }
            }
        }

        stage('Run Docker Container') {
            steps {
                script {
                    // Stop/remove old container if exists
                    sh "docker rm -f my-running-node-app || true"

                    // Run new container
                    sh "docker run -d -p 3000:3000 --name my-running-node-app my-node-app"
                }
            }
        }
    }
}

