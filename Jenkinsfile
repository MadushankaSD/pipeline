pipeline {
  agent any

  stages {
    stage('Build Spring Boot App') {
      steps {
        sh './mvnw clean package -DskipTests'
      }
    }

    stage('Docker Build & Tag') {
      steps {
        sh 'docker build -t madushanka7/myfirst-docker-repo:latest .'
        sh 'docker tag myjava1 $DOCKER_BFLASK_IMAGE'
      }
    }

    stage('Test Container') {
      steps {
        sh 'docker run --rm $IMAGE_NAME:$IMAGE_TAG'
      }
    }

    stage('Push to Docker Hub') {
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
      sh 'docker logout || true'
    }
  }
}
