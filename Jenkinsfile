pipeline {
    agent any

    environment {
        AWS_REGION = 'ap-south-1'   // Change to your AWS region
        ECR_REPO   = 'new-deployment' // Replace with your ECR repo name
        IMAGE_TAG  = "latest"
        ACCOUNT_ID = "new-deployment" // Replace with your AWS account ID
    }

    stages {
        stage('Checkout') {
            steps {
                checkout scm
            }
        }

        stage('Login to ECR') {
            steps {
                script {
                    sh """
                        aws ecr get-login-password --region $AWS_REGION \
                        | docker login --username AWS --password-stdin $ACCOUNT_ID.dkr.ecr.$AWS_REGION.amazonaws.com
                    """
                }
            }
        }

        stage('Build Docker Image') {
            steps {
                script {
                    sh "docker build -t $ECR_REPO:$IMAGE_TAG ."
                }
            }
        }

        stage('Tag Docker Image') {
            steps {
                script {
                    sh "docker tag $ECR_REPO:$IMAGE_TAG $ACCOUNT_ID.dkr.ecr.$AWS_REGION.amazonaws.com/$ECR_REPO:$IMAGE_TAG"
                }
            }
        }

        stage('Push to ECR') {
            steps {
                script {
                    sh "docker push $ACCOUNT_ID.dkr.ecr.$AWS_REGION.amazonaws.com/$ECR_REPO:$IMAGE_TAG"
                }
            }
        }
    }
}
