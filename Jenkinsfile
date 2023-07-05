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
        sh '''clean install

mvn clean compile'''
        echo 'Compilacion exitosa'
      }
    }

    stage('Test') {
      steps {
        sh 'mvn test '
        echo 'package exitoso'
      }
    }

    stage('Package') {
      steps {
        sh 'mvn package'
        echo 'package exitoso'
      }
    }

    stage('Deploy') {
      steps {
        echo 'Deploy exitoso'
      }
    }

  }
}