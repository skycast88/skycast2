pipeline {
    agent any
    environment {
        NODE_ENV = 'production'
        PORT = '3000'
        PATH = "C:\\Program Files\\nodejs;${env.PATH}"
        SONARQUBE_URL = 'http://localhost:9000'   // Replace with your SonarQube server URL
        SONARQUBE_TOKEN = credentials('d814a8a3-bada-4f74-a879-1243b619f0b3') // Reference to the SonarQube token (ensure this is configured in Jenkins' credentials store)
        SONAR_SCANNER_HOME = 'C:\\sonar-scanner'
        HOMEPATH = 'C:\\Users\\mohit.k.singh'
        PM2_HOME = 'C:\\Users\\mohit.k.singh\\.pm2'
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
        // stage('Quality Gate') {
        //     steps {
        //         script {
        //             // Wait for the analysis to finish and check the quality gate
        //             def qualityGate = waitForQualityGate()
        //             if (qualityGate.status != 'OK') {
        //                 error "SonarQube Quality Gate failed: ${qualityGate.status}"
        //             }
        //         }
        //     }
        // }

        stage('Run Performance Test') {
            steps {
                script {
                    echo 'Running performance test...'
                    bat 'artillery run performance/performance-test.yml --output performance/report.json'  // Adjust path if needed
                    //bat 'artillery report --output performance/report.html'  // Save the report
                    
                }
            }
        }

        stage('Run Security Audit') {
            steps {
                script {
                    echo 'Running npm audit...'
                    bat 'npm audit --json > npm-audit-report.json'
                    echo 'Generating npm audit report...'
                    bat 'npm audit'
                }
            }
        }

        

        
        stage('Start Server') {
            steps {
                // Run the server on localhost:3000
                script {
                   // echo 'Deploying Node.js application using PM2 on localhost...'
                   //  // Stop the current PM2 application if it exists
                   // // bat 'pm2 stop skycast || echo "PM2 process not found, starting a new one."'

                   //  // Start or restart the Node.js application using PM2
                   //  bat 'pm2 start app.js --name skycast'  // Assuming 'npm start' starts your app

                   //  // Optionally, save the PM2 process list to automatically restart on system reboot
                   //  bat 'pm2 save'
                    bat '''@echo off
                    pm2 list | findstr /i "skycast" > nul
                    if %errorlevel% equ 0 (
                        echo "Application 'skycast' is running, restarting it..."
                        pm2 restart skycast
                    ) else (
                        echo "Application 'skycast' is not running, starting it..."
                        pm2 start app.js --name skycast
                        pm2 save
                    )
                    '''
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
        always {
            archiveArtifacts artifacts: 'npm-audit-report.json, report.html', allowEmptyArchive: true
            archiveArtifacts artifacts: 'performance/report.json', allowEmptyArchive: true
        }
    }
}
