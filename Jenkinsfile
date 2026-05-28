pipeline {
    agent any

    environment {
        AWS_REGION = 'ap-south-1'          // Your AWS region
        ECR_REPO   = 'new-deployment'      // Your ECR repo name
        IMAGE_TAG  = "latest"              // You can use BUILD_NUMBER or commit hash
        ACCOUNT_ID = "848004113365"        // Your AWS account ID
    }

    stages {
        stage('Checkout') {
            steps {
                git branch: 'main',
                    url: 'https://github.com/manju230/new-deployment.git',
                    credentialsId: 'your-jenkins-cred-id'
            }
        }

        stage('Login to ECR') {
            steps {
                script {
                    sh """
                        aws ecr get-login-password --region ${AWS_REGION} \
                        | docker login --username AWS --password-stdin ${ACCOUNT_ID}.dkr.ecr.${AWS_REGION}.amazonaws.com
                    """
                }
            }
        }

        stage('Build Docker Image') {
            steps {
                script {
                    sh "docker build -t ${ECR_REPO}:${IMAGE_TAG} ."
                }
            }
        }

        stage('Tag Docker Image') {
            steps {
                script {
                    sh "docker tag ${ECR_REPO}:${IMAGE_TAG} ${ACCOUNT_ID}.dkr.ecr.${AWS_REGION}.amazonaws.com/${ECR_REPO}:${IMAGE_TAG}"
                }
            }
        }

        stage('Push to ECR') {
            steps {
                script {
                    sh "docker push ${ACCOUNT_ID}.dkr.ecr.${AWS_REGION}.amazonaws.com/${ECR_REPO}:${IMAGE_TAG}"
                }
            }
        }
    }
}
