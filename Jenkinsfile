pipeline {
    agent any

    stages {
        stage('Build Docker Image') {
            script {
                dockerapp = docker.build("KeyssonG/Jenkins:${env.BUILD_ID}", '-f ./src/Dockerfile ./src')
            }
        }

         stage('Push Docker Image') {
            script {
                docker.withRegistry('https://registry.hub.docker.com', 'dockerhub') {
                    dockerapp.push('latest')
                    dockerapp.push("${env.BUILD_ID}")
                }
            }
        }

        stage('Deploy no Kubernetes') {
            script {
                dockerapp = docker.build('KeyssonG/Jenkins:')
            }
        }
    }
}