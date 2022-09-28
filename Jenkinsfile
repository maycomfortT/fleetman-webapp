pipeline {
   agent any
   environment {
     // You must set the following environment variables
     // ORGANIZATION_NAME
     // YOUR_DOCKERHUB_USERNAME (it doesn't matter if you don't have one)
     
     SERVICE_NAME = "fleetman-webapp"
     REPOSITORY_TAG="${YOUR_DOCKERHUB_USERNAME}/${ORGANIZATION_NAME}-${SERVICE_NAME}:${BUILD_ID}"
     tag = sh(returnStdout: true, script: "git rev-parse --short=10 HEAD").trim()
     dockerhub=credentials('Docker')
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

      stage('Build Image') {
         steps {
           sh 'docker image build -t ${REPOSITORY_TAG} .'
         }
      }

      stage('Push and Tag Image to DockerHub') {
         steps {
   
sh 'echo $dockerhub_PSW | docker login -u $dockerhub_USR --password-stdin'
sh 'docker tag jenkins/jenkins:lts-jdk11 $dockerhub_USR/${REPOSITORY_TAG}'
sh 'docker push $dockerhub_USR/${REPOSITORY_TAG}'

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