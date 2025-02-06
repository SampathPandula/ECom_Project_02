pipeline {
    agent any
    environment {
        ECR_URI = "<ECR_URI>"
    }
    stages {
        stage('Checkout') {
            steps {
                git 'https://github.com/SampathPandula/ECom_Project_02.git'
            }
        }
        stage('Build Docker Image') {
            steps {
                sh 'docker build -t flask-app .'
                sh 'docker tag flask-app:latest $ECR_URI:latest'
            }
        }
        stage('Push to ECR') {
            steps {
                sh 'aws ecr get-login-password --region us-west-2 | docker login --username AWS --password-stdin $ECR_URI'
                sh 'docker push $ECR_URI:latest'
            }
        }
        stage('Deploy to Kubernetes') {
            steps {
                sh 'kubectl apply -f deployment.yaml'
                sh 'kubectl apply -f service.yaml'
            }
        }
    }
}
