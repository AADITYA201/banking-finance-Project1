pipeline {
    agent any

    environment {
        AWS_REGION = 'us-east-1'  // change if needed
        ECR_REPO =  '442426861623.dkr.ecr.us-east-1.amazonaws.com/financeme' // replace with your repo URL
        IMAGE_TAG = 'latest'
    }

    tools {
        maven 'Maven 3.8.5'
    }

    stages {
        stage('Clone Repo') {
            steps {
                git 'https://github.com/AADITYA201/banking-finance-Project1.git'
            }
        }

        stage('Build App') {
            steps {
                sh 'mvn clean package -DskipTests'
            }
        }

        stage('Build Docker Image') {
            steps {
                sh 'docker build -t financeme-app .'
            }
        }

        stage('Push to ECR') {
            steps {
                script {
                    sh '''
                        aws ecr get-login-password --region $AWS_REGION | docker login --username AWS --password-stdin $ECR_REPO
                    '''
                    sh '''
                        docker tag financeme-app:latest $ECR_REPO:$IMAGE_TAG
                    '''
                    sh '''
                        docker push $ECR_REPO:$IMAGE_TAG
                    '''
                }
            }
        }
    }
}
