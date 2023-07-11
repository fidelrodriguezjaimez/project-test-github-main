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
        sh 'mvn clean'
        echo 'Compilacion exitosa'
      }
    }

    stage('Test') {
      steps {
        sh 'nvn install'
        echo 'package exitoso'
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
          cd binaries
          pwd
          ls
        '''
        echo 'package exitoso'
      }
    }

    stage('SonarQube Scan') {
      steps {
        checkout scm
        sh "sonar-scanner \
                  -Dsonar.coveragePlugin=generic \
                  -Dsonar.projectKey=${SONAR_KEY} \
                  -Dsonar.host.url=${SONAR_SERVER} \
                  -Dsonar.login=${SONAR_TOKEN} \
                  -Dsonar.sources=src/main/java/com/furazin/projecttestgithub \
                  -Dsonar.sourceEncoding=UTF-8 \
                  -Dsonar.exclusions=src/main/java/com/furazin/projecttestgithub/main.java \
                  -Dsonar.tests=src/test/java \
                  -Dsonar.test.inclusions=src/test/java/*.java \
                  -Dsonar.java.binaries=build/classes/java/ \
                  -Dsonar.javascript.lcov.reportPaths=coverage/lcov.info"
        echo 'Scaneo Exitoso'
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
