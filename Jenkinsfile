pipeline {
    agent any

    tools {
        maven 'Maven 3.8.5' // Must match name in Jenkins "Global Tool Configuration"
    }

    stages {
        stage('Clone Repo') {
            steps {
                git 'https://github.com/AADITYA201/banking-finance-Project1.git'
            }
        }

        stage('Build') {
            steps {
                sh 'mvn clean package -DskipTests'
            }
        }

        stage('Docker Build') {
            steps {
                sh 'docker build -t financeme-app .'
            }
        }
    }
}
