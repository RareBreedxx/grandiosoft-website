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
                sh """
                sed -i 's|image: .*|image: ${IMAGE_NAME}:${IMAGE_TAG}|' ${K8S_DIR}/deployment.yaml
                """
            }
        }

        stage('Deploy to Kubernetes') {
            steps {
                sh """
                kubectl apply -f ${K8S_DIR}/deployment.yaml
                kubectl apply -f ${K8S_DIR}/service.yaml
                """
            }
        }

        stage('Verify Deployment') {
            steps {
                sh """
                kubectl rollout status deployment/grandiosoft-web
                kubectl get pods
                """
            }
        }
    }
}
