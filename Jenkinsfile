pipeline {
    agent any

    environment {
        AWS_REGION = "us-east-1"
        ECR_REPO = "123456789012.dkr.ecr.us-east-1.amazonaws.com/simple-devops-app"
        IMAGE_TAG = "latest"
    }

    stages {

        stage('Clone Repo') {
            steps {
                git 'https://github.com/yourusername/simple-devops-app.git'
            }
        }

        stage('Build Docker Image') {
            steps {
                sh 'docker build -t simple-app .'
            }
        }

        stage('Login to ECR') {
            steps {
                sh '''
                aws ecr get-login-password --region $AWS_REGION \
                | docker login --username AWS --password-stdin $ECR_REPO
                '''
            }
        }

        stage('Tag Image') {
            steps {
                sh 'docker tag simple-app:latest $ECR_REPO:$IMAGE_TAG'
            }
        }

        stage('Push to ECR') {
            steps {
                sh 'docker push $ECR_REPO:$IMAGE_TAG'
            }
        }

        stage('Deploy to Kubernetes') {
            steps {
                sh 'kubectl apply -f k8s/'
            }
        }
    }
}
