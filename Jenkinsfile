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
        sh 'docker build -t $DOCKER_REPO .'
        sh 'docker tag madushanka7/myfirst-docker-repo $DOCKER_REPO'
      }
    }

    stage('Push to Docker Hub') {
      steps {
          withCredentials([usernamePassword(credentialsId: "${DOCKER_CREDS}", passwordVariable: 'DOCKER_PASSWORD', usernameVariable: 'DOCKER_USERNAME')]) {
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
