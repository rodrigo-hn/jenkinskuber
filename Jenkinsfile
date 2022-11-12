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
                             --destination=rhndeveloper/myweb:${BUILD_NUMBER}
            '''
          }
        }
      }
    }

    stage('Deploy App to Kubernetes') {     
      steps {
        container('kubectl') {
          withCredentials([file(credentialsId: 'mykubeconfig', variable: 'KUBECONFIG')]) {
            sh 'sed -i "s/<TAG>/${BUILD_NUMBER}/" myweb.yaml'
            sh 'kubectl apply -f myweb.yaml'
            sh 'kubectl apply -f secret.yaml'
            sh 'kubectl apply -f mysql-pv.yaml'
            sh 'kubectl apply -f mysql-pvc.yaml'
            sh 'sleep 10 '
            sh 'kubectl apply -f deployment-mysql.yaml'
            sh 'kubectl apply -f svc-mysql.yaml'
          }
        }
      }
    }
  
  }
}