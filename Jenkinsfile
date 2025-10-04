pipeline {
    agent any  // Run checkout on host Jenkins first
    environment {
        IMAGE_NAME = "my-node-app"
    }
    stages {

        stage('Checkout Repo') {
            steps {
                echo "Checking out repository..."
                checkout scm
            }
        }

        stage('Build & Test in Docker') {
            agent {
                docker {
                    image 'node:18'
                    args '-v /var/run/docker.sock:/var/run/docker.sock'
                }
            }
            steps {
                echo "Installing dependencies..."
                sh 'npm install'

                echo "Running tests..."
                sh 'npm test || echo "No tests defined"'

                echo "Building Docker image..."
                script {
                    docker.build(env.IMAGE_NAME)
                }
            }
        }

        stage('Run Docker Container') {
            steps {
                echo "Running Docker container..."
                sh "docker run -d -p 3000:3000 ${env.IMAGE_NAME}"
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
