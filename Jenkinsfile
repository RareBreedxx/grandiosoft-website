pipeline {
    agent any

    environment {
        IMAGE_NAME = "grandiosoft-website"
        IMAGE_TAG  = "${BUILD_NUMBER}"
        K8S_DIR    = "site/k8s"
    }

    stages {

        stage('Build Docker Image') {
            steps {
                sh """
                docker build -t ${IMAGE_NAME}:${IMAGE_TAG} .
                """
            }
        }
        
        
        
        stage('Update Kubernetes Manifest') {
            steps {
                    sh '''
                    sed -i "s|image: .*|image: grandiosoft-website:${BUILD_NUMBER}|" site/k8s/deployment.yaml'''
            }
        }

        stage('Deploy to Kubernetes') {
            steps {
                sh """
                kubectl apply -f site/k8s/deployment.yaml
                kubectl apply -f site/k8s/service.yaml
                kubectl apply -f site/k8s/ingress.yaml
                """
            }
        }

        stage('Verify Deployment') {
            steps {
                sh """
                kubectl rollout status deployment/grandiosoft-web
                kubectl get pods
                kubectl get svc
                """
            }
        }
    }
}
