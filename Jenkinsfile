pipeline{
    agent any

    environment{
        AWS_REGION = 'us-east-1'
        IMAGE_ECR_REPO = '396608766727.dkr.ecr.us-east-1.amazonaws.com/femi-ci'
        ECR_REPO = '396608766727.dkr.ecr.us-east-1.amazonaws.com'
    }
    stages{
        stage('Trivy scan'){
            steps {
                sh 'trivy fs . -o scanreport.html'
                sh 'cat scanreport.html'
            }
        }
        stage('Docker Login'){
            steps {
                sh "aws ecr get-login-password --region $AWS_REGION | docker login --username AWS --password-stdin $ECR_REPO"
            }
        }
        stage('Docker build'){
            steps {
                sh 'docker build -t femi-ci .'
                sh 'docker build -t imageversion .'
            }
        }
        stage('Docker tag'){
            steps {
                sh "docker tag femi-ci:latest $IMAGE_ECR_REPO:latest"
                sh "docker tag imageversion $IMAGE_ECR_REPO:v1.$BUILD_NUMBER"
            }
        }
        stage('Docker push'){
            steps {
                sh "docker push $IMAGE_ECR_REPO:latest"
                sh "docker push $IMAGE_ECR_REPO:v1.$BUILD_NUMBER"
            }
        }
    }
}