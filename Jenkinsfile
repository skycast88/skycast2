pipeline {
    agent any
    environment {
        PATH = "C:\\Program Files\\nodejs;${env.PATH}"
    }
    stages {
        stage('Checkout') {
            steps {
                // Clone the repository from Git
                checkout([
                        $class: 'GitSCM',
                        branches: [[name: 'main']],
                        userRemoteConfigs: [[
                            url: 'https://github.com/skycast88/skycast2.git',    
                            credentialsId: 'd438b28d-8ef3-4d75-b3ec-e5247bb025f3' // Replace with your Jenkins credentials ID
                        ]]
                    ])
               
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
        stage('SonarQube Analysis') {
            def scannerHome = tool 'SonarScanner';
                withSonarQubeEnv() {
                  bat "${scannerHome}/bin/sonar-scanner"
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
