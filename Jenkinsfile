pipeline {
  agent {
    node {
      label 'Node agent'
    }

  }
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