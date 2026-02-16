pipeline {
  agent any

  environment {
    IMAGE_NAME = "rarebreedxx/grandiosoft-website"
    IMAGE_TAG  = "${BUILD_NUMBER}"
    KUBE_DEPLOY = "grandiosoft-web"
  }

  stages {

    stage('Build Docker Image') {
      steps {
        sh """
          docker build -t ${IMAGE_NAME}:${IMAGE_TAG} .
        """
      }
    }

    stage('Push to Docker Hub') {
      steps {
        withCredentials([usernamePassword(
          credentialsId: 'dockerhub-creds',
          usernameVariable: 'DOCKER_USER',
          passwordVariable: 'DOCKER_PASS'
        )]) {
          sh """
            echo "$DOCKER_PASS" | docker login -u "$DOCKER_USER" --password-stdin
            docker push ${IMAGE_NAME}:${IMAGE_TAG}
          """
        }
      }
    }

    stage('Deploy to Kubernetes') {
      steps {
        sh """
          kubectl set image deployment/${KUBE_DEPLOY} web=${IMAGE_NAME}:${IMAGE_TAG}
          kubectl rollout status deployment/${KUBE_DEPLOY}
        """
      }
    }
  }
}

