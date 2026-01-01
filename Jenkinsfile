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
        sh '''
        docker ps -q --filter "name=grandiosoft" | xargs -r docker rm -f
        '''
    }
}

        stage('Run New Container') {
            steps {
                sh 'docker run -d -p 8081:80 --name grandiosoft grandiosoft-website'
            }
        }
    }
}
