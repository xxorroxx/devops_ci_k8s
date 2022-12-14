pipeline {
  
  agent {
    kubernetes {
          containerTemplate(
      name: 'kubectl', 
      image: 'amaceog/kubectl',
      resourceRequestCpu: '100m',
      resourceLimitCpu: '300m',
      resourceRequestMemory: '300Mi',
      resourceLimitMemory: '500Mi', 
      ttyEnabled: true, 
      command: 'cat'
    )
      yaml '''
        apiVersion: v1
        kind: Pod
        spec:
          containers:
          - name: kubectl 
            image: amaceog/kubectl
            resourceRequestCpu: 100m
            resourceLimitCpu: 300m
            resourceRequestMemory: 300Mi
            resourceLimitMemory: 500Mi 
            ttyEnabled: true 
            command: 
          - cat
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
          - name: docker
            image: docker:dind
            tty: true
            securityContext:
              privileged: true
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
