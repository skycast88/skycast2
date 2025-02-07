pipeline {
    agent any
    environment {
        PATH = "C:\\Program Files\\nodejs;${env.PATH}"
    }
    stages {
        stage('Install Dependencies') {
            steps {
                bat 'npm install'
            }
        }
        stage('Build') {
            steps {
                bat 'npm run build'
            }
        }
        stage('Deploy Locally') {
            steps {
                script {
                    // Start the app in the background
                    bat 'start /B npm start'
                    
                    // Wait for the app to initialize
                    bat 'ping 127.0.0.1 -n 10 > nul'
                    
                    // Verify the app is running on port 3000
                    bat 'curl http://localhost:3000 || echo "Application did not start successfully"'
                }
            }
        }
    }
}
