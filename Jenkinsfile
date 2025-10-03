pipeline {
    agent any

    stages {
        stage('Build App') {
            steps {
                sh 'npm install'
            }
        }
        stage('Test App') {
            steps {
                sh 'echo "No tests yet"'
            }
        }
        stage('Build Docker Image') {
            steps {
                sh 'docker build -t sample-app:latest .'
            }
        }
        stage('Run Docker Container') {
            steps {
                sh 'docker run -d -p 3000:3000 --name sample-app-container sample-app:latest'
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
