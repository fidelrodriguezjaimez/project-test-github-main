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

    stage('Build') {
      steps {
        sh 'mvn -ntp -B clean verify -DskipITs=true'
        
        sh '''#!/bin/bash
          echo "This is start $(pwd)"
          ls
          cd target
          pwd
          ls
          cd classes
          pwd
          ls'''
         echo 'Compilacion exitosa'
      }
    }

    stage('SonarQube Scan') {
      steps {
        checkout scm
        sh 'pwd'
        sh "sonar-scanner \
                -Dsonar.projectKey=${SONAR_KEY} \
                -Dsonar.host.url=${SONAR_SERVER} \
                -Dsonar.login=${SONAR_TOKEN} \
                -Dsonar.sources=src \
                -Dsonar.sourceEncoding=UTF-8 \
                -Dsonar.exclusions=*.properties\
                -Dsonar.tests=src/test \
                -Dsonar.java.source=8 \
                -Dsonar.java.binaries=target/classes \
                -Dsonar.java.libraries=/home/jenkins/.m2/**/*.jar"
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
    SONAR_TOKEN = credentials('sonartoken-secret')
    HARBOR_URL = 'demo.goharbor.io'
    HARBOR_USERNAME = 'fidel.rodriguez'
    HARBOR_PASSWORD = credentials('harborpas-secret')
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
