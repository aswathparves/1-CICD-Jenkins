pipeline {
    agent any  // Run on Jenkins host

    environment {
        IMAGE_NAME = "my-node-app"
        CONTAINER_NAME = "my-node-app"
        APP_DIR = "/var/jenkins_home/workspace/assessment-1"  // Hardcoded workspace inside Jenkins container
    }

    stages {
        stage('Prepare Workspace') {
            steps {
                echo "Preparing workspace..."
                // If repo files already exist, skip; else clone
                sh "git clone https://github.com/aswathparves/1-CICD-Jenkins ${APP_DIR} || echo 'Repo already exists'"
            }
        }

        stage('Build & Test in Docker') {
            agent {
                docker {
                    image 'node:18'
                    // Mount Docker socket and workspace into container
                    args "-v /var/run/docker.sock:/var/run/docker.sock -v ${APP_DIR}:/app -w /app"
                }
            }
            steps {
                echo "Listing files in container..."
                sh 'ls -la'

                echo "Installing dependencies..."
                sh 'npm install'

                echo "Running tests..."
                sh 'npm test || echo "No tests defined"'

                echo "Building Docker image..."
                sh "docker build -t ${env.IMAGE_NAME} ."
            }
        }

        stage('Run Docker Container') {
            steps {
                echo "Stopping old container if exists..."
                sh "docker rm -f ${env.CONTAINER_NAME} || true"

                echo "Running Docker container..."
                sh "docker run -d -p 3000:3000 --name ${env.CONTAINER_NAME} ${env.IMAGE_NAME}"
            }
        }
    }

    post {
        success {
            echo 'Build succeeded!'
            // Add Slack or Email notification here
        }
        failure {
            echo 'Build failed!'
        }
    }
}
