pipeline{
    agent any

    stages{
        stage('Trivy scan'){
            steps {
                sh 'trivy fs . -o scanreport.html'
                sh 'cat scanreport.html'
            }
        }
        stage('Docker Login'){
            steps {
                sh 'aws ecr get-login-password --region us-east-1 | docker login --username AWS --password-stdin 396608766727.dkr.ecr.us-east-1.amazonaws.com'
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
                sh 'docker tag femi-ci:latest 396608766727.dkr.ecr.us-east-1.amazonaws.com/femi-ci:latest'
                sh "docker tag imageversion 396608766727.dkr.ecr.us-east-1.amazonaws.com/imageversion:v1.$BUILD_NUMBER"
            }
        }
        stage('Docker push'){
            steps {
                sh 'docker push 396608766727.dkr.ecr.us-east-1.amazonaws.com/femi-ci:latest'
                sh "396608766727.dkr.ecr.us-east-1.amazonaws.com/jenkins-ciimageversion:v1.$BUILD_NUMBER"
            }
        }
    }
}