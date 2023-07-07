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
        sh 'mvn clean compile'
        echo 'Compilacion exitosa'
      }
    }

    stage('Test') {
      steps {
        sh 'mvn test'
        echo 'package exitoso'
      }
    }

    stage('Package') {
      steps {
        sh 'mvn package'
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
                  -Dsonar.tests=src/test \
                  -Dsonar.test.inclusions=*.spec.ts \
                  -Dsonar.typescript.lcov.reportPaths=coverage/lcov.info"
        echo 'Scaneo Exitoso'
      }
    }

    stage('BuildImage') {
      steps {
        sh 'docker build -t java-imagen .'
        echo 'Build Image succes'
      }
    }

    stage('Push Harbor') {
      steps {
        sh 'docker tag java-imagen demo.goharbor.io/jenkinsjavaimage/java-imagen:BUILD_NUMBER'
        sh 'docker login demo.goharbor.io'
        sh 'docker push login demo.goharbor.io/jenkinsjavaimage/java-imagen:BUILD_NUMBER'
        echo 'Image Push succed'
      }
    }

  }
  environment {
    SONAR_KEY = '23_Coppel_TestJenkinsGitHub'
    SONAR_SERVER = 'https://devtools.axity.com/sonarlts'
    SONAR_TOKEN = '7eb678e177fe0d360298d0e466cd5177166a9b6e'
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
