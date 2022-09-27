pipeline {
   agent any
   environment {
     // You must set the following environment variables
     // ORGANIZATION_NAME
     // YOUR_DOCKERHUB_USERNAME (it doesn't matter if you don't have one)
     
     SERVICE_NAME = "fleetman-webapp"
     REPOSITORY_TAG="${YOUR_DOCKERHUB_USERNAME}/${ORGANIZATION_NAME}-${SERVICE_NAME}:${BUILD_ID}"
       tag = sh(returnStdout: true, script: "git rev-parse --short=10 HEAD").trim()
   }

   stages {
      stage('Preparation') {
         steps {
            cleanWs()
            git credentialsId: 'GitHub', url: "https://github.com/${ORGANIZATION_NAME}/${SERVICE_NAME}"
         }
      }
      stage('Build') {
         steps {
            sh 'echo No build required for Webapp.'
         }
      }

      stage('Build and Push Image') {
         steps {
            // sh 'docker login -u aubriellepie -p Mario219!!! '
            // sh 'docker pull devapp:tested'
            // sh 'docker tag devapp:tested my-app:v0.1'
            // sh 'docker push devapp:v0.1'
           // sh 'echo No build required for Webapp.'
           sh 'docker image build -t ${REPOSITORY_TAG} .'
           sh 'echo ${tag}'
            // withCredentials([usernamePassword(credentialsId: 'Docker')]) {
            //         sh "docker push ${REPOSITORY_TAG}:${tag}"
            //     }
sh 'docker logout'
sh 'docker login -u=aubriellepie -p=Mario219!!!'
sh 'docker tag ${tag}'
sh 'docker push ${REPOSITORY_TAG}'

         }
      }
      stage('Deploy to Cluster') {        
          steps {
            withKubeConfig([credentialsId: '9e72e27c-4c21-48eb-bced-4435c9c366c4']) {
                   sh 'envsubst < ${WORKSPACE}/deploy.yaml | kubectl apply -f -'
               }         
          }
      }
   }
}