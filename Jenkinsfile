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
RUN <<EOF
npm install
EOF

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

    }
}

