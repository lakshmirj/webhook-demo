pipeline {
    agent any
    environment {
        registry = "197984982305.dkr.ecr.us-east-2.amazonaws.com/jenkins-docker-demo"
        AWS_DEFAULT_REGION="us-east-2"
    }
    stages {
        stage ('git checkout') {
            steps {
                checkout([$class: 'GitSCM', branches: [[name: '*/master']], extensions: [], userRemoteConfigs: [[url: 'https://github.com/lakshmirj/webhook-demo.git']]])
            }
        }
        stage ('build docker'){
            steps {
                script {
                    dockerImage = docker.build registry
                }
            }
        }
        stage ('login to ecr'){
            steps {
                script {
                    sh 'aws ecr get-login-password --region us-east-2 | docker login --username AWS --password-stdin 197984982305.dkr.ecr.us-east-2.amazonaws.com'
                }
            }
        }
        stage ('build and push') {
            steps {
                script {
                    sh 'docker build -t jenkins-docker-demo .'
                    sh 'docker tag jenkins-docker-demo:latest 197984982305.dkr.ecr.us-east-2.amazonaws.com/jenkins-docker-demo:latest'
                    sh 'docker push 197984982305.dkr.ecr.us-east-2.amazonaws.com/jenkins-docker-demo:latest'
                }
            }
        }
        stage ('run docker') {
            steps {
                script {
                    sh 'docker run -d -p 8096:8080 --rm --name jenkins-docker-demo 197984982305.dkr.ecr.us-east-2.amazonaws.com/jenkins-docker-demo:latest'
                }
            }
        }
    }
}
