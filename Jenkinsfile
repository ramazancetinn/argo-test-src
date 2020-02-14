node{
  agent none
  stage('Build') {
  environment {
    DOCKERHUB_CREDS = credentials('dockerhub')
  }
  steps {
        container('docker') {
          // Build new image
          sh "until docker ps; do sleep 3; done && docker build -t ramazancetin/argocd-demo:${env.BUILD_ID} ."
          // Publish new image
          sh "docker login --username $DOCKERHUB_CREDS_USR --password $DOCKERHUB_CREDS_PSW && docker push ramazancetin/argocd-demo:${env.BUILD_ID}"
        }
      }
  }
}