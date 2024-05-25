pipeline {
    agent any

    environment {
        AWS_ACCOUNT_ID="851725291001"
        AWS_DEFAULT_REGION="us-east-1"
        IMAGE_REPO_NAME="my-php"
        IMAGE_TAG="v1"
        REPOSITORY_URI = "851725291001.dkr.ecr.us-east-1.amazonaws.com/my-php"
    }

    stages {
        stage('Cloning Git') {
            steps {
                checkout([$class: 'GitSCM', branches: [[name: '*/main']], doGenerateSubmoduleConfigurations: false, extensions: [], submoduleCfg: [], userRemoteConfigs: [[credentialsId: '', url: 'https://github.com/kannancloud001/my-php-task.git']]])     
            }
        }
        stage('Build Docker Image') {
            steps {
                script {
                    // Build the Docker image
                    dockerImage = docker.build "${IMAGE_REPO_NAME}:${IMAGE_TAG}"
                }
            }
        }
        stage('Login to AWS ECR') {
            steps {
                script {
                    // Login to AWS ECR
                    sh '''
                    aws ecr get-login-password --region $AWS_DEFAULT_REGION | docker login --username AWS --password-stdin 851725291001.dkr.ecr.us-east-1.amazonaws.com/my-php
                    '''
                }
            }
        }
        stage('Push Docker Image to ECR') {
            steps {
                script {
                    // Push the Docker image to AWS ECR
                    sh '''
                    docker push 851725291001.dkr.ecr.us-east-1.amazonaws.com/my-php
                    '''
                }
            }
        }
    }
}
