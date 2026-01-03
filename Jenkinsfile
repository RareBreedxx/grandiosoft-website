pipeline {
    agent any

    parameters {
        choice(
            name: 'TARGET_ENV',
            choices: ['dev', 'prod'],
            description: 'Deployment environment'
        )
    }

    environment {
        IMAGE_NAME = "grandiosoft-website"
        CONTAINER_NAME = "grandiosoft-${TARGET_ENV}"
        PORT = "${TARGET_ENV == 'prod' ? '8081' : '8082'}"
        ENVIRONMENT = "${TARGET_ENV}"
    }

    stages {

        stage('Build Docker Image') {
            steps {
                sh "docker build -t ${IMAGE_NAME}:${BUILD_NUMBER} ."
            }
        }

        stage('Cleanup Old Container') {
            steps {
                sh "docker ps -q --filter name=${CONTAINER_NAME} | xargs -r docker rm -f"
            }
        }

        stage('Run New Container') {
            steps {
                sh "docker run -d -p ${PORT}:80 --name ${CONTAINER_NAME} --restart unless-stopped -e ENVIRONMENT=${ENVIRONMENT} ${IMAGE_NAME}:${BUILD_NUMBER}"
            }
        }
    }
}
