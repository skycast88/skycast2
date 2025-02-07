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
        
        stage('SonarQube Analysis') {
            steps {
                script {
                    // Run SonarQube scanner for code analysis
                    withSonarQubeEnv('LocalSonarQube') {  // 'Local SonarQube' is the name of your configured SonarQube instance in Jenkins
                        bat """
                            %SONAR_SCANNER_HOME%\\bin\\sonar-scanner.bat ^
                            -Dsonar.projectKey=com.skycast ^
                            -Dsonar.projectName="Skycast" ^
                            -Dsonar.projectVersion=1.0 ^
                            -Dsonar.sources=.
                            -Dsonar.host.url=%SONARQUBE_URL% ^
                            -Dsonar.login=%SONARQUBE_TOKEN%
                        """
                    }
                }
            }
        }
        stage('Start Server') {
            steps {
                // Run the server on localhost:3000
                script {
                   bat 'start /B set NODE_ENV=production && set PORT=3000 && npm start'  // This will run 'node server.js' if 'start' script is in package.json
                }
            }
        }
    }
}
