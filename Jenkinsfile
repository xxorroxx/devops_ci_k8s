pipeline {
  
  agent {
    kubernetes {
      label 'sample-app'
      defaultContainer 'jnlp'
      yaml """
apiVersion: v1
kind: Pod
metadata:
labels:
  component: ci
spec:
  # Use service account that can deploy to all namespaces
  serviceAccountName: cd-jenkins
  containers:
  - name: maven
    image: maven:alpine
    command:
  - cat
    tty: true
  - name: node
    image: node:16-alpine3.12
    command:
  - cat
    tty: true
}
  }
    
  stages {
    stage('clone repository') {
      steps {
         container('maven') {
            sh '''
                  java -version
                  mvn --version
               '''
        }
      }
    }

    stage('Deploy billing App') {
      steps {
            sh 'kubectl --server https://192.168.49.2:8443 --insecure-skip-tls-verify=true apply -f deployment-billing-app-back-jenkins.yaml '
          }

        }
      }

    }
  
