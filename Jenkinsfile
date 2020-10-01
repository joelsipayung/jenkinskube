pipeline {

  environment {
    registry = "joelsipayung/jenkinskube"
    dockerImage = ""
  }

  agent any

  stages {

    stage('Checkout Source') {
      steps {
        git 'https://github.com/joelsipayung/jenkinskube.git'
      }
    }

    stage('Build image') {
      steps{
        script {
          dockerImage = docker.build registry + ":$BUILD_NUMBER"
        }
      }
    }

    stage('Push Image') {
      steps{
        script {
          docker.withRegistry( "" ) {
            dockerImage.push()
          }
        }
      }
    }
    
    stage('Apply Kubernetes files') {
      kubeconfig(credentialsId: 'mykubeconfig', serverUrl: 'https://usw1.kubesail.com') {
    // some block
        sh 'kubectl deploy -f myweb.yaml'
      }
    }

  }

}
