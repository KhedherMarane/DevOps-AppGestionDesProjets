pipeline {
  agent any

  environment {
    DOCKERHUB_USER = 'maranekhedher'
  }

  stages {
    stage('Checkout') {
      steps {
        checkout scm
      }
    }

    stage('Build images') {
      steps {
        sh 'docker compose build'
      }
    }

        stage('Push images') {
      steps {
        withCredentials([usernamePassword(credentialsId: 'dockerhub',
                                          usernameVariable: 'DH_USER',
                                          passwordVariable: 'DH_PASS')]) {
          retry(3) {
            sh 'echo $DH_PASS | docker login -u $DH_USER --password-stdin'
            sh 'docker push $DOCKERHUB_USER/gestion-backend:latest'
            sh 'docker push $DOCKERHUB_USER/gestion-frontend:latest'
          }
        }
      }
    }

    stage('Deploy') {
      steps {
        sh 'docker compose up -d'
      }
    }
  }

  post {
    always {
      sh 'docker logout || true'
    }
  }
}
