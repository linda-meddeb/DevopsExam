pipeline {
    agent any
    tools {
        nodejs 'nodejs'
        dockerTool 'docker'
    }

    stages {
        stage('Checkout from Git') {
            steps {
                git branch: 'main', url: 'https://github.com/linda-meddeb/DevopsExam.git'
            }
        }
    
        stage("Build") {
            steps {
                script {
                    docker.build("linda-meddeb/todo-devops:${env.BUILD_NUMBER}")
                }
            }
        }
        stage("Push") {
            steps {
                script {
                    docker.withRegistry('https://registry.hub.docker.com', 'docker-hub-credentials') {
                        docker.image("linda-meddeb/todo-devops:${env.BUILD_NUMBER}").push()
                        docker.image("linda-meddeb/todo-devops:${env.BUILD_NUMBER}").push("latest")
                    }
                }
            }
        }

        stage("Deploy") {
            steps {
                sh "kubectl apply -f k8/"
            }
        }
    }
}
