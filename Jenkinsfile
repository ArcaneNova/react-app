pipeline {
    agent any

    environment {
        DOCKERHUB_CREDENTIALS = credentials('dockerhub-creds') 
    }

    stages {
        stage('Checkout Code') {
            steps {
                 url: 'https://github.com/ArcaneNova/react-app.git',
                 git branch: 'main'
            }
        }

        stage('Build Docker Image') {
            steps {
                script {
                    sh 'docker build -t arshadnoor585/myreact-app:latest .'
                }
            }
        }

        stage('Push Docker Image') {
            steps {
                script {
                    withCredentials([usernamePassword(credentialsId: 'dockerhub-creds', usernameVariable: 'DOCKER_USERNAME', passwordVariable: 'DOCKER_PASSWORD')]) {
                        sh 'echo $DOCKER_PASSWORD | docker login --username $DOCKER_USERNAME --password-stdin'
                        sh 'docker push arshadnoor585/myreact-app:latest'
                    }
                }
            }
        }
    }

    post {
        success {
            echo "Build and Push successful!"
        }
        failure {
            echo "Build or Push failed."
        }
    }
}
