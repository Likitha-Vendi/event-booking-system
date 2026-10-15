
pipeline {
    agent any

    options {
        timestamps()
        disableConcurrentBuilds()
    }

    environment {
        COMPOSE_FILE = 'docker-compose.yml'
        COMPOSE = '"C:\\Users\\user\\AppData\\Local\\Programs\\DockerDesktop\\resources\\cli-plugins\\docker-compose.exe"'
    }

    stages {
        stage('Checkout') {
            steps {
                checkout scm
                bat 'git log -1 --oneline'
            }
        }

        stage('Validate Docker Compose') {
            steps {
                bat 'docker --version'
                bat '%COMPOSE% -f %COMPOSE_FILE% config --quiet'
            }
        }

        stage('Build Backend Image') {
            steps {
                bat 'docker build -t event-booking-backend:ci -f server/Dockerfile server'
            }
        }

        stage('Build Frontend Image') {
            steps {
                bat 'docker build -t event-booking-frontend:ci -f client/Dockerfile client'
            }
        }

        stage('Verify Images') {
            steps {
                bat 'docker image inspect event-booking-backend:ci'
                bat 'docker image inspect event-booking-frontend:ci'
            }
        }
    }

    post {
        success {
            echo 'SUCCESS: Checkout, Compose validation, and both Docker image builds completed.'
        }
        failure {
            echo 'FAILED: Review the first failed stage in Console Output.'
        }
    }
}
