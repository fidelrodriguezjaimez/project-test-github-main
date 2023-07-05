pipeline {
  agent any
  stages {
    stage('Git CheckOut') {
      agent any
      steps {
        git(url: 'https://github.com/fidelrodriguezjaimez/project-test-github-main.git', branch: 'master')
        echo 'CheckOut realizado con exito'
      }
    }

    stage('Compilado') {
      steps {
        sh 'npm install'
        echo 'Compilacion exitosa'
      }
    }

    stage('Test') {
      steps {
        sh './jenkins/scripts/test.sh '
        echo 'package exitoso'
      }
    }

    stage('deploy') {
      steps {
        sh 'echo "Deploying....."'
        echo 'Deploy exitoso'
      }
    }

  }
}