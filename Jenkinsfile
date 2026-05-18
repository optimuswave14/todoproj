pipeline {
    agent any

    environment {
        IMAGE_NAME = "deeptiii/todoproj"
    }

    stages {

        stage('Clone Repository') {
            steps {
                git 'https://github.com/optimuswave14/todoproj.git'
            }
        }

        stage('SonarQube Analysis') {
            steps {
                withSonarQubeEnv('sonarcloud') {
                    bat 'sonar-scanner'
                }
            }
        }

        stage('Trivy Scan') {
            steps {
                bat 'trivy fs . > trivy-report.txt'
            }
        }

        stage('Build Docker Image') {
            steps {
                bat 'docker build -t %IMAGE_NAME% .'
            }
        }

        stage('Push Docker Image') {
            steps {
                withCredentials([usernamePassword(
                    credentialsId: 'dockerhub-creds',
                    usernameVariable: 'DOCKER_USER',
                    passwordVariable: 'DOCKER_PASS'
                )]) {

                    bat 'docker login -u %DOCKER_USER% -p %DOCKER_PASS%'
                    bat 'docker push %IMAGE_NAME%'
                }
            }
        }
    }
}