pipeline {
    agent any
    stages{
        stage('CodeScan'){
            steps{
             sh 'trivy fs . -o result.html'
             sh 'cat result.html'
        }
        }
        stage('Docker login'){
            steps{
                sh 'aws ecr get-login-password --region us-east-1 | \
                docker login --username AWS \
                --password-stdin 051826737816.dkr.ecr.us-east-1.amazonaws.com'

            }
        }
    stage('dockerImageBuild'){
        steps{
            sh 'docker build -t jenkins-cicd .'
            
        }
    }
    stage('dokerImageTag'){
        steps{
            sh 'docker tag jenkins-cicd:latest\
             051826737816.dkr.ecr.us-east-1.amazonaws.com/jenkins-cicd:latest'
             sh 'docker tag imageversion\
             051826737816.dkr.ecr.us-east-1.amazonaws.com/jenkins-cicd:v1.$BUILD_NUMBER'
        }
    }
    stage('pushImage'){
        steps{
            sh 'docker push 051826737816.dkr.ecr.us-east-1.amazonaws.com/jenkins-cicd:latest'
            sh 'docker push 051826737816.dkr.ecr.us-east-1.amazonaws.com/jenkins-cicd:v1.$BUILD_NUMBER'
        }
    }
    }
}