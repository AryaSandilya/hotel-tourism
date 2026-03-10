pipeline {
    agent any

    environment {
        IMAGE_NAME = "tourism-app"
        IMAGE_TAG = "latest"
    }

    stages {

        stage('Clone Repository') {
           steps {
               git branch: 'main', url: 'https://github.com/AryaSandilya/hotel-tourism.git'
           }
       }

        stage('Build Maven Project') {
            steps {
                sh 'mvn clean package'
            }
        }

        stage('Build Docker Image') {
            steps {
                sh 'docker build -t tourism-app:latest .'
            }
        }

        stage('Load Image to KIND') {
            steps {
                sh 'kind load docker-image tourism-app:latest --name dev-cluster'
            }
        }

        stage('Deploy to Kubernetes') {
            steps {
                sh 'kubectl rollout restart deployment tourism-deployment'
            }
        }

    }
}