pipeline {
    agent any

    stages {
        stage('Cleanup') {
            steps {
                script {
                    // Вбиваємо всі процеси, що слухають порт 80
                    sh 'sudo fuser -k 80/tcp || true'
                    
                    // Зупиняємо і видаляємо всі контейнери, що використовують порт 80
                    sh '''
                    docker ps -q --filter "publish=80" | xargs -r docker stop
                    docker ps -aq --filter "publish=80" | xargs -r docker rm
                    '''
                }
            }
        }

        stage('Build Docker Image') {
            steps {
                sh 'docker build -t nginx/custom:latest .'
            }
        }

        stage('Run Container') {
            steps {
                script {
                    sh 'docker run -d --name my_nginx -p 80:80 nginx/custom:latest'
                }
            }
        }
    }
}
