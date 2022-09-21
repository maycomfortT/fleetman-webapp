pipeline {
   agent any

   environment {
     // You must set the following environment variables
     // ORGANIZATION_NAME
     // YOUR_DOCKERHUB_USERNAME (it doesn't matter if you don't have one)
     
     SERVICE_NAME = "fleetman-webapp"
     REPOSITORY_TAG="${YOUR_DOCKERHUB_USERNAME}/${ORGANIZATION_NAME}-${SERVICE_NAME}:${BUILD_ID}"
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
         }
      }
      stage('List Pods') {
           steps {
               withKubeConfig([credentialsId: 'GitHub'],serverUrl:'https://127.0.0.1:50678') {
                   sh 'curl -LO "https://storage.googleapis.com/kubernetes-release/release/v1.20.5/bin/linux/amd64/kubectl"'  
                   sh 'chmod u+x ./kubectl'
                 //  sh '/usr/local/bin/kubectl apply -f '  
                   sh './kubectl get pods'
               }
           }
       }
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
             // sh 'echo No build required for Webapp.'
         // sh 'envsubst < ${WORKSPACE}/deploy.yaml | kubectl apply -f -'
        // sh 'kubectl apply -f ${WORKSPACE}/deploy.yaml'
            sh 'envsubst < ${WORKSPACE}/deploy.yaml | kubectl apply -f -'
          }
      }
   }
}