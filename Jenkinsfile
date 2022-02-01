pipeline {
    environment {
        registry = "naniece19/kubernetes"
        registryCredential = 'dockerhub_id'
        dockerImage = ''
    }
    agent any
    stages {
        stage('Cloning our Git') {
            steps {
                git 'https://github.com/Narayanad419/dockertest1.git'
            }
        }
        stage('Building our image') {
            steps {
                script {
                    dockerImage = docker.build registry + ":v$BUILD_NUMBER"
                }
            }
        }
        stage('Push Image To DockerHUB') {
            steps {
                script {
                    docker.withRegistry( '', registryCredential ) {
                        dockerImage.push()
                    }
                }
            }
        }
        stage('Cleaning up') {
            steps {
                sh "docker rmi $registry:v$BUILD_NUMBER"
            }
        }
        stage('Deploying to K8S') {
            steps {
                sh "ls -al"
                sh "kubectl apply -f k8sdeploy.yml"
                sh "kubectl set image deployment/nginx-deployment nginx=$registry:v$BUILD_NUMBER --record"
            }
        }
        
        }
    }
}
