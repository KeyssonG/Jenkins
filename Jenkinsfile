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
            environment {
                tag_version = "${env.build_id}"
            }
            steps {
                withKubeConfig([credentialsId: 'kubeconfig']) {

                    bat """
                        powershell -Command "(Get-Content ./k8s/deployment.yaml) -replace '{{tag}}', '${tag_version}' | Set-Content ./k8s/deployment.yaml"
                    """


                    bat 'kubectl apply -f ./k8s/deployment.yaml'
                }
            }
        }
    }
}
