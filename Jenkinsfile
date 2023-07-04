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
        sh 'echo "Compiling Code"'
        echo 'Compilacion exitosa'
      }
    }

    stage('Package') {
      steps {
        sh 'echo "Packaging...."'
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