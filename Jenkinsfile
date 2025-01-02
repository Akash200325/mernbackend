pipeline {
    agent any

    environment {
        // Replace with the actual Node.js installation directory if required
        PATH = "C:\\Program Files\\nodejs;${env.PATH}" 
        SONAR_HOST_URL = 'http://localhost:9000' // SonarQube server URL
        SONAR_AUTH_TOKEN = 'sqp_d3d422e5b17debe26656759a733d94471564010f' // SonarQube token
    }

    stages {
        stage('Setup Environment') {
            steps {
                echo 'Verifying Node.js and npm installation...'
                bat 'node --version' // Check Node.js version
                bat 'npm --version'  // Check npm version
            }
        }

        stage('Checkout Code') {
            steps {
                echo 'Cloning the Git repository...'
                checkout scm
            }
        }

        stage('Install Dependencies') {
            steps {
                echo 'Installing dependencies from package.json...'
                bat 'npm install'
            }
        }

        stage('SonarQube Analysis') {
            steps {
                echo 'Running SonarQube analysis...'
                bat """
                sonar-scanner.bat ^
                    -D"sonar.projectKey=mernbackend" ^
                    -D"sonar.sources=." ^
                    -D"sonar.host.url=http://localhost:9000" ^
                    -D"sonar.token=sqp_d3d422e5b17debe26656759a733d94471564010f"
                """
            }
        }

        stage('Archive and Publish Results') {
            steps {
                echo 'Archiving and publishing analysis results...'
                archiveArtifacts artifacts: '**/sonarqube-report.json', allowEmptyArchive: true
            }
        }
    }

    post {
        always {
            echo 'Pipeline execution completed!'
        }
        success {
            echo 'Pipeline completed successfully!'
        }
        failure {
            echo 'Pipeline failed. Please check the logs for errors.'
        }
    }
}
