pipeline {
    agent any
    environment {
        DOCKER_IMAGE = "rahulraj41/hotel-tourism"
        // Force kubectl to use your working config
        KUBECONFIG = 'C:/Users/khana/.kube/config' 
    }
    stages {
        stage('Clone Repository') {
            steps {
                git branch: 'main', url: 'https://github.com/AryaSandilya/hotel-tourism.git'
            }
        }

        stage('Build Maven Project') {
            steps {
                // Use 'bat' for Windows
                bat 'mvn clean package'
            }
        }

        stage('Build Docker Image') {
            steps {
                // In 'bat', we use %DOCKER_IMAGE% for environment variables
                bat "docker build -t %DOCKER_IMAGE%:v2 ."
            }
        }

        stage('Push Docker Image') {
            steps {
                withCredentials([usernamePassword(credentialsId: 'rahulraj41', usernameVariable: 'USER', passwordVariable: 'PASS')]) {
                    bat """
                    docker login -u %USER% -p %PASS%
                    docker push %DOCKER_IMAGE%:v2
                    """
                }
            }
        }

        stage('Deploy to Kubernetes') {
		    steps {
		        // Use --kubeconfig to point directly to your working file
		        bat 'kubectl --kubeconfig C:\\Users\\khana\\.kube\\config set image deployment/tourism-deployment tourism-container=%DOCKER_IMAGE%:v2'
		    }
		}
    }
}