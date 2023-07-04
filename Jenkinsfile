pipeline {
  agent any
  stages {
    stage('Build') {
      agent {
        node {
          label 'uno'
        }

      }
      steps {
        sh 'npm install'
      }
    }

  }
}