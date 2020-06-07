pipeline {
    agent any
    stages {
        stage("Build") {
          environment {
            DOCKERHUB_CREDS = credentials('dockerhub')
          }
          steps {
            sh "until docker ps; do sleep 3; done && docker build -t ramazancetin/hellonode:${env.GIT_COMMIT} ."
            sh "docker login --username $DOCKERHUB_CREDS_USR --password $DOCKERHUB_CREDS_PSW && docker push ramazancetin/hellonode:${env.GIT_COMMIT}"
          }
        }
        stage('Deploy') {
            steps {
                sh "rm -rf argo-test-deploy || true"
                sh "git clone https://github.com/ramazancetinn/argo-test-deploy.git"
                sh "git config --global user.email 'ci@ci.com'"
                input message:'Approve deployment?'
              dir("argo-test-deploy"){
                sh "cd ./prod && kustomize edit set image ramazancetin/hellonode:${env.GIT_COMMIT}"
                withCredentials([usernamePassword(credentialsId: 'git', passwordVariable: 'GIT_CREDS_PSW', usernameVariable: 'GIT_CREDS_USR')]) {
                    sh("git commit -am 'Publish new version' && git push https://${GIT_CREDS_USR}:${GIT_CREDS_PSW}@github.com/ramazancetinn/argo-test-deploy.git")
                }
              }
            }
        }
    }
}
