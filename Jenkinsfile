pipeline {
    agent any
    environment {
        PATH = "C:\\Program Files\\nodejs;${env.PATH}"
    }
    stages {
        stage('Checkout') {
            steps {
                // Clone the repository from Git
                git 'https://github.com/your-repository.git'  // Replace with your repo URL
            }
        }
        stage('Install Dependencies') {
            steps {
                // Install Node.js dependencies using npm
                script {
                    bat 'npm install'  // Windows batch command for npm install
                }
            }
        }
        stage('Start Server') {
            steps {
                // Run the server on localhost:3000
                script {
                    bat 'npm start'  // This will run 'node server.js' if 'start' script is in package.json
                }
            }
        }
    }
}
