pipeline {
    agent any
environment{
    AWS_REGION = 'us-east-1'
    IMAGE_ECR_REPO = '051826737816.dkr.ecr.us-east-1.amazonaws.com/jenkins-cicd'
    ECR_REPO = '051826737816.dkr.ecr.us-east-1.amazonaws.com'

}
    stages{
        stage('CodeScan'){
            steps{
             sh 'trivy fs . -o result.html'
             sh 'cat result.html'
        }
        }
        stage('Docker login'){
            steps{
                sh "aws ecr get-login-password --region $AWS_REGION | \
                docker login --username AWS \
                --password-stdin $ECR_REPO"

            }
        }
    stage('dockerImageBuild'){
        steps{
            sh 'docker build -t jenkins-cicd .'
            sh 'docker build -t imageversion . '
        }
    }
    stage('dokerImageTag'){
        steps{
            sh "docker tag jenkins-cicd:latest\
             $IMAGE_ECR_REPO:latest"
             sh "docker tag imageversion\
             $IMAGE_ECR_REPO:v1.$BUILD_NUMBER"
        }
    }
    stage('pushImage'){
        steps{
            sh "docker push $IMAGE_ECR_REPO:latest"
            sh "docker push $IMAGE_ECR_REPO:v1.$BUILD_NUMBER"
        }
    }
    }
}