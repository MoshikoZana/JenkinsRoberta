pipeline {
    agent any

    environment {
        ECR_URL = 'public.ecr.aws/r7m7o9d4'
    }

    stages {
        stage('Build and Push to ECR') {
            steps {
                script {
                    // Log in to ECR
                    sh 'aws ecr-public get-login-password --region us-east-1 | docker login --username AWS --password-stdin $ECR_URL'

                    // Build and push Docker image
                    sh "docker build -t $ECR_URL/robberta:0.0.${BUILD_NUMBER} ."
                    sh "docker push $ECR_URL/robberta:0.0.${BUILD_NUMBER}"
                }
            }
        }
    }

    post {
        always {
            // Clean up Docker images
            script {
                sh 'docker image prune -a --force'
            }
        }
    }
}
