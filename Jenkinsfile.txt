pipeline {
    agent any
    
    environment {
        DOCKERHUB_CREDENTIALS = credentials('dockerhub-creds')
        DOCKER_IMAGE = "gokulkr610/hello-minikube"
    }

    stages {
        
        stage('Clone Repo') {
            steps {
                git branch: 'main',
                url: 'https://github.com/YOUR_GITHUB_USERNAME/YOUR_REPO_NAME.git'
            }
        }

        stage('Build Docker Image') {
            steps {
                script {
                    sh 'docker build -t $DOCKER_IMAGE .'
                }
            }
        }

        stage('Login to DockerHub') {
            steps {
                script {
                    sh "echo $DOCKERHUB_CREDENTIALS_PSW | docker login -u $DOCKERHUB_CREDENTIALS_USR --password-stdin"
                }
            }
        }

        stage('Push Image') {
            steps {
                script {
                    sh 'docker push $DOCKER_IMAGE'
                }
            }
        }
    }
}
