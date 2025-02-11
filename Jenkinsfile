pipeline {
    agent any
    environment {
        NODE_ENV = 'production'
        PORT = '3000'
        PATH = "C:\\Program Files\\nodejs;${env.PATH}"
        SONARQUBE_URL = 'http://localhost:9000'   // Replace with your SonarQube server URL
        SONARQUBE_TOKEN = credentials('d814a8a3-bada-4f74-a879-1243b619f0b3') // Reference to the SonarQube token (ensure this is configured in Jenkins' credentials store)
        SONAR_SCANNER_HOME = 'C:\\sonar-scanner'
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
        stage('Build') {
            steps {
            
                bat 'npm run build'
               
            }
        }
        
        stage('Start Server') {
            steps {
                // Run the server on localhost:3000
                script {
                   echo 'Deploying Node.js application using PM2 on localhost...'
                    // Stop the current PM2 application if it exists
                   // bat 'pm2 stop skycast || echo "PM2 process not found, starting a new one."'

                    // Start or restart the Node.js application using PM2
                    bat 'pm2 start app.js --name skycast'  // Assuming 'npm start' starts your app

                    // Optionally, save the PM2 process list to automatically restart on system reboot
                    bat 'pm2 save'
                }
            }
        }
    }
    post {
        success {
            echo 'Deployment was successful!'
        }
        failure {
            echo 'Deployment failed!'
        }
    }
}
