pipeline {
    agent any   // Jenkins can run this pipeline on any available agent

    stages {
        stage('Clone Repository') {
            steps {
                // Pull latest code from your GitHub repo
                git 'https://github.com/<your-username>/1-CICD-Jenkins.git'
            }
        }

        stage('Install Dependencies') {
            steps {
                // Install Node.js dependencies
                sh 'npm install'
            }
        }

        stage('Run Tests') {
            steps {
                // Run tests (for now just a placeholder)
                sh 'npm test'
            }
        }

        stage('Build Docker Image') {
            steps {
                // Build Docker image from Dockerfile
                sh 'docker build -t sample-app:latest .'
            }
        }

        stage('Run Docker Container') {
            steps {
                // Stop & remove old container if it exists, then run new one
                sh '''
                docker stop sample-app-container || true
                docker rm sample-app-container || true
                docker run -d -p 3000:3000 --name sample-app-container sample-app:latest
                '''
            }
        }
    }

    post {
        success {
            echo '✅ Build and deployment succeeded!'
        }
        failure {
            echo '❌ Build failed!'
        }
    }
}
