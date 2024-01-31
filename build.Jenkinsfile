pipeline {
    agent any

    environment {
        ECR_URL = 'public.ecr.aws/r7m7o9d4'
    }

    stages {
        stage('Build and Push to ECR') {
            steps {
                script {
                    // Get ECR Public authentication token
                    def ecrAuth = sh(script: "aws ecr-public get-login-password --region us-east-1", returnStdout: true).trim()

                    // Docker login to ECR
                    sh "echo $ecrAuth | docker login --username AWS --password-stdin $ECR_URL"

                    // Build and push Docker image
                    sh "docker build -t $ECR_URL/robberta:0.0.${BUILD_NUMBER} ."
                    sh "docker push $ECR_URL/robberta:0.0.${BUILD_NUMBER}"
                }
            }
        }
    }

    post {
        always {
            script {
                sh 'docker image prune -a --force'
            }
        }
    }
}
