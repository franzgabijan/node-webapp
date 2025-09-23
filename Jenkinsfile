pipeline {
    agent any

    stages {
        stage('Build Docker Image') {
            steps {
                script {
                    // Create a Dockerfile dynamically from the provided instructions
                    writeFile file: 'Dockerfile', text: '''
FROM node:lts-slim
EXPOSE 3000
WORKDIR /home/node/app
RUN npm install
COPY . /home/node/app
CMD ["npm", "start"]
                    '''
                    // Build the Docker image
                    sh 'docker build -t my-node-app .'
                }
            }
        }

        stage('Run Docker Container') {
            steps {
                script {
                    // Run the Docker container
                    sh 'docker run -d -p 3000:3000 --name my-running-node-app my-node-app'
                }
            }
        }

        stage('Clean Up') {
            steps {
                script {
                    // Stop and remove the container after the pipeline finishes
                    sh 'docker stop my-running-node-app || true'
                    sh 'docker rm my-running-node-app || true'
                }
            }
        }
    }

    post {
        always {
            // Ensure cleanup even if stages fail
            script {
                sh 'docker stop my-running-node-app || true'
                sh 'docker rm my-running-node-app || true'
            }
        }
    }
}

