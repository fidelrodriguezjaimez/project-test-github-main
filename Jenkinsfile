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
        bat 'Compile.bat'
        echo 'Compilacion exitosa'
      }
    }

    stage('Package') {
      steps {
        bat 'Package.bat'
        echo 'package exitoso'
      }
    }

    stage('deploy') {
      steps {
        bat 'Deploy.bat'
        echo 'Deploy exitoso'
      }
    }

  }
}