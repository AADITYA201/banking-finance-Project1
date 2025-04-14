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
                  withCredentials([usernamePassword(credentialsId: 'aws-ecr-creds', usernameVariable: 'AWS_ACCESS_KEY_ID', passwordVariable: 'AWS_SECRET_ACCESS_KEY')]) {
            sh '''
                aws configure set aws_access_key_id $AWS_ACCESS_KEY_ID
                aws configure set aws_secret_access_key $AWS_SECRET_ACCESS_KEY
                aws ecr get-login-password --region us-east-1 | docker login --username AWS --password-stdin 442426861623.dkr.ecr.us-east-1.amazonaws.com/financeme
                docker tag financeme:latest 442426861623.dkr.ecr.us-east-1.amazonaws.com/financeme:latest
                docker push 442426861623.dkr.ecr.us-east-1.amazonaws.com/financeme:latest
                    '''
                }
            }
        }
    }
}
