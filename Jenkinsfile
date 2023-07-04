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
        sh 'pwd'
      }
    }

  }
}