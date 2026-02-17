pipeline {
  agent any

  environment {
    APP_DIR = "site"
    IMAGE   = "grandiosoft-site"
    TAG     = "ci-${BUILD_NUMBER}"
  }

  stages {
    stage("Checkout") {
      steps {
        checkout scm
      }
    }

    stage("Build Image") {
      steps {
        dir("${APP_DIR}") {
          sh """
            docker build -t ${IMAGE}:${TAG} .
          """
        }
      }
    }

    stage("Smoke Test (Container serves HTTP)") {
      steps {
        dir("${APP_DIR}") {
          sh """
            set -e
            docker rm -f ${IMAGE}-test || true
            docker run -d --name ${IMAGE}-test -p 18081:80 ${IMAGE}:${TAG}

            # Wait briefly then test HTTP 200
            sleep 2
            curl -fsS http://localhost:18081/ >/dev/null

            docker rm -f ${IMAGE}-test
          """
        }
      }
    }
  }

  post {
    always {
      sh "docker image ls | head -n 20"
      sh "docker rm -f ${IMAGE}-test || true"
    }
  }
}
