pipeline {
  agent any

  stages {
    stage('Build') {
      steps {
        sh 'docker build -t myjava1 .'
        sh 'docker tag myjava1 $DOCKER_BFLASK_IMAGE'
      }
    }
    stage('Test') {
      steps {
        sh 'docker run myjava1'
      }
    }
    stage('Deploy') {
      steps {
        withCredentials([usernamePassword(credentialsId: "${DOCKER}", passwordVariable: 'DOCKER_PASSWORD', usernameVariable: 'DOCKER_USERNAME')]) {
          sh "echo \$DOCKER_PASSWORD | docker login -u \$DOCKER_USERNAME --password-stdin docker.io"
          sh 'docker push $DOCKER_REPO'
        }
      }
    }
  }
  post {
    always {
      sh 'docker logout'
    }
  }
}