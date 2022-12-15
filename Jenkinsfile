pipeline {
  
  agent {
    kubernetes {
      yaml '''
        apiVersion: v1
        kind: Pod
        spec:
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
        '''
    }
  }
    
  stages {
    stage('clone repository') {
      steps {
         container('maven') {
            sh '''
                  java -version
                  mvn --version
                  git --version
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
  
