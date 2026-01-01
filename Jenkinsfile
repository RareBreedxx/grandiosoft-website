pipeline {
    agent any

    environment {
        IMAGE_NAME = "grandiosoft-website"
        CONTAINER_NAME = "grandiosoft"
        PORT = "8081"
    }

    stages {
        stage('Build Image') {
            steps {
                sh 'docker build -t $IMAGE_NAME .'
            }
        }

        stage('Cleanup Old Container') {
            steps {
                sh '''
                docker ps -q --filter "name=$CONTAINER_NAME" | xargs -r docker rm -f
                '''
            }
        }

        stage('Deploy') {
            steps {
                sh 'docker run -d -p $PORT:80 --name $CONTAINER_NAME $IMAGE_NAME'
            }
        }
    }
}

