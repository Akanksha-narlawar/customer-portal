pipeline {
    agent any

    environment {
        IMAGE_NAME = "customer-portal"
        CONTAINER_NAME = "customer-portal-test"
        APP_PORT = "5000"
        HOST_PORT = "8085"
    }

    stages {

        stage('Checkout') {
            steps {
                checkout scm
            }
        }

        stage('Build') {
    steps {
        bat 'C:\\Users\\akank\\AppData\\Local\\Programs\\Python\\Python311\\python.exe -m venv venv'
        bat 'venv\\Scripts\\python.exe -m pip install -r requirements.txt'
    }
}

stage('Test') {
    steps {
        bat 'venv\\Scripts\\python.exe -m pytest'
    }
}

        stage('Docker Build') {
            steps {
                bat 'docker build -t %IMAGE_NAME%:build-%BUILD_NUMBER% .'
            }
        }

        stage('Container Verification') {
            steps {
                bat 'docker run -d --name %CONTAINER_NAME% -p %HOST_PORT%:%APP_PORT% %IMAGE_NAME%:build-%BUILD_NUMBER%'
                bat 'timeout /t 5'
                bat 'curl.exe -f http://localhost:%HOST_PORT%/health'
            }
        }

        stage('Cleanup') {
            steps {
                bat 'docker stop %CONTAINER_NAME% || exit /b 0'
                bat 'docker rm %CONTAINER_NAME% || exit /b 0'
            }
        }
    }
}