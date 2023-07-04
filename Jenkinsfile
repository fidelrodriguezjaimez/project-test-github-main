pipeline {
  agent any
  stages {
    stage('uno') {
      agent {
        node {
          label 'uno'
        }

      }
      steps {
        sh 'echo 1'
      }
    }

  }
}