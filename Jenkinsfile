pipeline {
    agent any

    stages {
        stage('Cleanup') {
            steps {
                script {
                    sh "sudo fuser -k 80/tcp || true"
                }
            }
        }

        stage('Checkout Code') {
            steps {
                git branch: 'main', url: 'https://github.com/artem-turko/PRIKM.git'
            }
        }

        stage('Build Docker Image') {
            steps {
                script {
                    sh 'docker build -t nginx/custom:latest .'
                }
            }
        }

        stage('Run Container') {
            steps {
                script {
                    sh 'docker stop my_nginx || true && docker rm my_nginx || true'
                    sh 'docker run -d --name my_nginx -p 80:80 nginx/custom:latest'
                }
            }
        }
    }
}
