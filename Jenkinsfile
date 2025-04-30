pipeline {
    agent any

    environment {
        DOCKER_IMAGE = "artemturko/prikm"
    }

    stages {
        stage('Start') {
            steps {
                echo 'Lab_2: started by GitHub'
            }
        }

        stage('Image build') {
            steps {
                script {
                    sh "docker build -t prikm:latest ."

                    // Створюємо 4 теги
                    sh "docker tag prikm ${DOCKER_IMAGE}:latest"
                    sh "docker tag prikm ${DOCKER_IMAGE}:build-${BUILD_NUMBER}"
                    sh "docker tag prikm ${DOCKER_IMAGE}:lab2"
                    sh "docker tag prikm ${DOCKER_IMAGE}:git-${GIT_COMMIT}"
                }
            }
        }

        stage('Push to registry') {
            steps {
                withDockerRegistry([ credentialsId: "dockerhub_token", url: "" ]) {
                    sh "docker push ${DOCKER_IMAGE}:latest"
                    sh "docker push ${DOCKER_IMAGE}:build-${BUILD_NUMBER}"
                    sh "docker push ${DOCKER_IMAGE}:lab2"
                    sh "docker push ${DOCKER_IMAGE}:git-${GIT_COMMIT}"
                }
            }
        }

        stage('Deploy image') {
            steps {
                // Видаляємо попередній контейнер, якщо існує
                sh "docker rm -f prikm-container || true"

                // Запускаємо з тегом latest
                sh "docker run -d -p 80:80 --name prikm-container ${DOCKER_IMAGE}:latest"
            }
        }
    }
}
