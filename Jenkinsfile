pipeline {
   agent any
   environment {
     // You must set the following environment variables
     // ORGANIZATION_NAME
     // YOUR_DOCKERHUB_USERNAME (it doesn't matter if you don't have one)
     
     SERVICE_NAME = "fleetman-webapp"
     REPOSITORY_TAG="${YOUR_DOCKERHUB_USERNAME}/${ORGANIZATION_NAME}-${SERVICE_NAME}:${BUILD_ID}"
      //tag = sh(returnStdout: true, script: "git rev-parse --short=10 HEAD").trim()
      gitParameter name: 'TAG',
                   type: 'PT_TAG',
                   defaultValue: 'master'
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
           // sh 'echo No build required for Webapp.'
           sh 'docker image build -t ${REPOSITORY_TAG} .'
          // sh 'docker compose  -f ${WORKSPACE}/test.yaml up' 

         }
      }
      // stage('List Pods') {
      //      steps {
      //          withKubeConfig([credentialsId: 'config', serverUrl: 'https://127.0.0.1:53859']) {
      //              sh 'curl -LO "https://storage.googleapis.com/kubernetes-release/release/v1.20.5/bin/linux/amd64/kubectl"'
      //              sh 'chmod u+x ./kubectl'
      //              sh 'chmod 777 kubectl'
      //              sh 'mv ./kubectl /usr/local/bin'
      //              sh './kubectl get pods -n dev'
      //          }
      //      }
      // }
         //  stage('List Pods') {
         //   steps {
         //       withKubeConfig([credentialsId: 'GitHub']) {
         //           sh 'curl -LO "https://dl.k8s.io/release/$(curl -L -s https://dl.k8s.io/release/stable.txt)/bin/linux/amd64/kubectl"'
         //           sh 'install -o root -g root -m 0755 kubectl /usr/local/bin/kubectl'
         //           sh 'chmod +x kubectl'
         //           sh 'mkdir -p ~/.local/bin'
         //           sh 'mv ./kubectl ./kubectl /usr/local/bin'
         //           //sh 'kubectl auth can-i get secret --as upbound-cloud-impersonator'
         //           sh 'kubectl version'
         //       }
         //   }
      // }
      stage('Deploy to Cluster') {        
          steps {
            withKubeConfig([credentialsId: '9e72e27c-4c21-48eb-bced-4435c9c366c4']) {
                   sh 'envsubst < ${WORKSPACE}/deploy.yaml | kubectl apply -f -'
               }         
          }
      }
   }
}