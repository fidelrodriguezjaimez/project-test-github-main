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
        sh 'mvn clean install'
        echo 'Compilacion exitosa'
      }
    }

    stage('Package') {
      steps {
        sh 'mvn package'
        echo 'package exitoso'
      }
    }

    stage('folder') {
      steps {
        sh script:'''
          #!/bin/bash
          echo "This is start $(pwd)"
          ls
          cd target
          pwd
          ls
          cd classes
          pwd
          ls
          cd com
          pwd
          ls
        '''
        echo 'package exitoso'
      }
    }

    stage('Análisis de SonarQube') {
      steps {
        withSonarQubeEnv('sonarqube') {
          sh 'sonar-scanner'
        }
      }
    }

    stage('Generar informe de JaCoCo') {
      steps {
        jacoco(execPattern: 'target/**/*.exec')
      }
    }

    stage('BuildImage') {
      steps {
        sh 'docker build -t demo.goharbor.io/jenkinsjavaimage/java-imagen:${BUILD_NUMBER} .'
        echo 'Build Image succes'
      }
    }

    stage('Push Harbor') {
      steps {        
        sh 'docker login ${HARBOR_URL} -u ${HARBOR_USERNAME} -p ${HARBOR_PASSWORD}'
        sh 'docker push ${HARBOR_URL}/jenkinsjavaimage/java-imagen:${BUILD_NUMBER}'
        sh 'docker rmi ${HARBOR_URL}/jenkinsjavaimage/java-imagen:${BUILD_NUMBER}'
        sh 'docker images'
        echo 'Image Push succed'
      }
    }

  }
  environment {
    SONAR_KEY = '23_Coppel_TestJenkinsGitHub'
    SONAR_SERVER = 'https://devtools.axity.com/sonarlts'
    SONAR_TOKEN = credentials("sonartoken-secret")
    HARBOR_URL = 'demo.goharbor.io'
    HARBOR_USERNAME = 'fidel.rodriguez'
    HARBOR_PASSWORD = credentials("harborpas-secret")
  }
  post {
    success {
      echo 'Esto se ejecutará solo si se ejecuta correctamente'
    }

    failure {
      echo 'Esto se ejecutará solo si falla'
    }

  }
}
