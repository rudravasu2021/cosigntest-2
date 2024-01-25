pipeline {
  agent any
  environment {
    GITHUB_TOKEN=credentials('github-token')
    IMAGE_NAME='rudravasu2021/cosigntest-v2'
    IMAGE_VERSION='2.2'
    COSIGN_PASSWORD=credentials('cosignpassword1')
    COSIGN_PRIVATE_KEY=credentials('cosign-privatekey')
  }
  stages {

    stage('build image') {
      steps {
        sh 'docker build -t $IMAGE_NAME:$IMAGE_VERSION .'
      }
    }
    stage('login to GHCR') {
      steps {
        sh 'echo "$GITHUB_TOKEN_PSW" | docker login ghcr.io -u $GITHUB_TOKEN_USR --password-stdin'
      }
    }
    stage('tag image') {
      steps {
        sh 'docker tag $IMAGE_NAME:$IMAGE_VERSION ghcr.io/$IMAGE_NAME:$IMAGE_VERSION'
      }
    }
    stage('push image') {
      steps {
        sh 'docker push ghcr.io/$IMAGE_NAME:$IMAGE_VERSION'
      }
    }
    stage('sign the container image') {
      steps {
        sh 'cosign version'
        sh 'cosign sign --key $COSIGN_PRIVATE_KEY ghcr.io/$IMAGE_NAME:$IMAGE_VERSION'
      }
    }
  }
  post {
    always {
      sh 'docker logout'
    }
  }
}
