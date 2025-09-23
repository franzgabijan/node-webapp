pipeline {
    agent {
        label 'linux-node' 
    }

    options {
        skipDefaultCheckout() 
    }

    stages {
        stage('Install Docker on Agent') {
            when {
               
                expression { !fileExists('/usr/bin/docker') && !fileExists('/usr/local/bin/docker') }
            }
            steps {
                script {
                  
                    sh 'sudo apt-get update && sudo apt-get install -y docker.io'
                   
                    sh 'sudo usermod -aG docker jenkins' 
                    sh 'sudo service docker start' 
                }
            }
        }

        stage('Build Docker Image') {
            steps {
                script {
                    
                    def dockerfileContent = """
FROM node:lts-slim
EXPOSE 3000
WORKDIR /home/node/app
RUN npm install
COPY . /home/node/app
CMD ["npm", "start"]
"""
                    writeFile file: 'Dockerfile', text: dockerfileContent

                    // Build the Docker image
                    sh "docker build -t my-node-app ."
                }
            }
        }

        stage('Run Docker Container') {
            steps {
                script {
                    // Run the Docker container
                    sh "docker run -d -p 3000:3000 --name my-running-node-app my-node-app"
                }
            }
        }

        stage('Clean Up') {
            steps {
                script {
                    // Stop and remove the container after the build (optional)
                    sh 'docker stop my-running-node-app || true'
                    sh 'docker rm my-running-node-app || true'
                    // Remove the Docker image (optional)
                    sh 'docker rmi my-node-app || true'
                }
            }
        }
    }
}

