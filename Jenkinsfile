pipeline {
    agent any

    environment {
        DOCKER_HUB_USERNAME = 'yashsuman' // <-- change this to your Docker Hub username
        IMAGE_NAME = 'nodejsdocker1'
        IMAGE_TAG = 'latest'
        TF_WORKING_DIR = '.'
    }

    stages {
        stage('Checkout') {
            steps {
                git branch: 'main', url: 'https://github.com/Yashsuman04/Blog-Website.git'
            }
        }

        stage('Install Node.js Dependencies') {
            steps {
                    bat 'npm install'
            }
        }

        stage('Build Docker Image') {
            steps {
                    bat "docker build -t %DOCKER_HUB_USERNAME%/%IMAGE_NAME%:%IMAGE_TAG% ."
            }
        }

        stage('Docker Hub Login') {
            steps {
                withCredentials([usernamePassword(credentialsId: 'docker-hub-credentials', usernameVariable: 'DOCKERHUB_USER', passwordVariable: 'DOCKERHUB_PASS')]) {
                    bat "echo %DOCKERHUB_PASS% | docker login -u %DOCKERHUB_USER% --password-stdin"
                }
            }
        }

        stage('Push Docker Image to Docker Hub') {
            steps {
                bat "docker push %DOCKER_HUB_USERNAME%/%IMAGE_NAME%:%IMAGE_TAG%"
            }
        }
    }

    post {
        success {
            echo 'All stages completed successfully!'
        }
        failure {
            echo 'Build failed.'
        }
    }
}
