pipeline {
    agent any

    stages {
        stage('Checkout') {
            steps {
                git url: 'https://github.com/gokulkr610/aws-devops-assessment.git',
                    branch: 'main',
                    credentialsId: 'github-creds'
            }
        }

        stage('Build Image') {
            steps {
                script {
                    dockerImage = docker.build("gokulkr610/aws-devops-assessment")
                }
            }
        }

        stage('Login to DockerHub') {
            steps {
                script {
                    docker.withRegistry('', 'dockerhub-creds') {
                        echo "Logged in"
                    }
                }
            }
        }

        stage('Push Image') {
            steps {
                script {
                    docker.withRegistry('', 'dockerhub-creds') {
                        dockerImage.push()
                    }
                }
            }
        }
    }
}

