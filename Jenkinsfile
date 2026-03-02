pipeline {
    agent any

    stages {
        stage('Checkout') {
            steps {
                git branch: 'master',
                    url: 'https://github.com/Roh9324/jobportal.git'
            }
        }

        stage('Build') {
            steps {
                bat 'mvn clean package -DskipTests'
            }
        }

        stage('Stop Old App') {
            steps {
                bat '''
                    FOR /F "tokens=5" %%P IN ('netstat -ano ^| findstr :9090') DO taskkill /PID %%P /F
                    exit 0
                '''
            }
        }

        stage('Deploy') {
            steps {
                bat 'start /B java -jar target\\*.jar --server.port=9090'
            }
        }
    }

    post {
        success { echo '✅ Live at http://localhost:9090' }
        failure { echo '❌ Build failed!' }
    }
}
