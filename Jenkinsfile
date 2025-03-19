pipeline {
    agent any

    stages {
        stage('Cleanup') {
            steps {
                script {
                    sh 'sudo fuser -k 80/tcp || true'
                    sh '''
                    sudo netstat -tulnp | grep :80 && echo "Port 80 is still in use!" || echo "Port 80 is free."
                    '''
                    sh '''
                    docker ps -q --filter "publish=80" | xargs -r docker stop
                    docker ps -aq --filter "publish=80" | xargs -r docker rm
                    '''
                    sh 'docker ps -a'
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
