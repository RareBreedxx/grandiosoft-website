pipeline {
    agent any

    stages {
        stage('Build Docker Image') {
            steps {
                sh 'docker build -t grandiosoft-website .'
            }
        }

        stage('Stop Old Container') {
            steps {
                sh 'docker rm -f grandiosoft || true'
            }
        }

        stage('Run New Container') {
            steps {
                sh 'docker run -d -p 8081:80 --name grandiosoft grandiosoft-website'
            }
        }
    }
}
