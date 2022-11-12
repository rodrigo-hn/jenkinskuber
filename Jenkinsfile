pipeline {

  options {
    ansiColor('xterm')
  }

  agent {
    kubernetes {
      yamlFile 'builder.yaml'
    }
  }

  stages {

    stage('Kaniko Build & Push Image') {
      steps {
        container('kaniko') {
          script {
            sh '''
            /kaniko/executor --dockerfile `pwd`/Dockerfile \
                             --context `pwd` \
                             --destination=rhndeveloper/myweb:latest
            '''
          }
        }
      }
    }

    stage('Deploy App to Kubernetes') {     
      steps {
        container('kubectl') {
          withCredentials([file(credentialsId: 'mykubeconfig', variable: 'KUBECONFIG')]) {
            sh 'kubectl apply -f myweb.yaml'
            sh 'kubectl apply -f secret.yaml'
            sh 'kubectl apply -f mysql-pv.yaml'
            sh 'kubectl apply -f mysql-pvc.yaml'
            sh 'kubectl apply -f deployment-mysql.yaml'
            sh 'kubectl apply -f svc-mysql.yaml'
          }
        }
      }
    }
  
  }
}