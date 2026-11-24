pipeline {
    agent any

    environment {
        DOCKERHUB_USER = 'ayajguirim'
        PATH = "C:\\Users\\jguir\\AppData\\Local\\Programs\\DockerDesktop\\resources\\bin;${env.PATH}"
    }

    stages {
        stage('Checkout') {
            steps {
                git branch: 'main',
                    credentialsId: 'github-token',
                    url: 'https://github.com/jguirimaya24-create/MyAppReact.git'
            }
        }

        stage('Build images') {
            steps {
                bat 'docker build -t %DOCKERHUB_USER%/mern-server:latest ./Server'
                bat 'docker build -t %DOCKERHUB_USER%/mern-client:latest ./Client'
            }
        }

        stage('Push to Docker Hub') {
            steps {
                withCredentials([usernamePassword(credentialsId: 'dockerhub-creds', usernameVariable: 'DH_USER', passwordVariable: 'DH_PASS')]) {
                    bat 'echo %DH_PASS%| docker login -u %DH_USER% --password-stdin'
                    bat 'docker push %DOCKERHUB_USER%/mern-server:latest'
                    bat 'docker push %DOCKERHUB_USER%/mern-client:latest'
                }
            }
        }

        stage('Run containers') {
            steps {
                bat 'docker-compose down'
                bat 'docker-compose up -d --build'
                bat 'docker ps'
            }
        }
    }
}