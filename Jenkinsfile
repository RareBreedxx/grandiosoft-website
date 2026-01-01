pipeline {
    agent any

    environment {
        IMAGE_NAME = "grandiosoft-website"
        CONTAINER_NAME = "grandiosoft"
        PORT = "8081"
    }

    stages {

        stage('Build Docker Image') {
            steps {
                sh '''
                docker build -t ${IMAGE_NAME}:${BUILD_NUMBER} .
                '''
            }
        }

        stage('Cleanup Old Container') {
            steps {
                sh '''
                docker ps -q --filter "name=${CONTAINER_NAME}" | xargs -r docker rm -f
                '''
            }
        }

        stage('Run New Container') {
            steps {
                sh '''
                docker run -d -p ${PORT}:80 \
                --name ${CONTAINER_NAME} \
                --restart unless-stopped \
                ${IMAGE_NAME}:${BUILD_NUMBER}
                '''
            }
        }
    }
}
