pipeline {
    agent any

    stages {
        stage('Build Docker Image') {
            steps {
                script {
                    dockerapp = docker.build("keyssong/jenkins:${env.build_id}", '-f ./src/Dockerfile ./src')
                }
            }
        }

        stage('Push Docker Image') {
            steps {
                script {
                    docker.withRegistry('https://registry.hub.docker.com', 'dockerhub') {
                        dockerapp.push('latest')
                        dockerapp.push("${env.build_id}")  
                    }
                }
            }
        }

        stage('Deploy no Kubernetes') {
            steps {
                bat 'echo executando o comando kubectl apply'
            }
        }
    }
}
