pipeline {
  agent any
    environment {
    // General configurations
    SONAR_KEY = "23_Coppel_TestJenkinsGitHub";
    SONAR_SERVER = "https://devtools.axity.com/sonarlts";
    SONAR_TOKEN = "7eb678e177fe0d360298d0e466cd5177166a9b6e";
  }
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
        sh 'javac -d opt/javaBuildClass src/main/java/**/**/**/main.java src/main/java/**/**/**/Math/Arithmetic.java'
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
        // Checkout your code repository
        checkout scm
    
        // Run the SonarQube scanner with the sonarqube-generic-coverage plugin
        sh "sonar-scanner \
          -Dsonar.coverageReportPaths=coverage.xml \
          -Dsonar.coveragePlugin=generic \
          -Dsonar.genericCoverage.reportPaths=coverage.xml \
          -Dsonar.projectKey=${SONAR_KEY} \
          -Dsonar.host.url=${SONAR_SERVER} \
          -Dsonar.login=${SONAR_TOKEN} \
          -Dsonar.sources=. \
          -Dsonar.java.binaries=opt/javaBuildClass \
          -Dsonar.exclusions=src/main/java/com/furazin/projecttestgithub/main.java \
          -Dsonar.tests=src/test \
          -Dsonar.test.inclusions=**/*.spec.ts"        
      }
    }

    stage('BuildImage') {
      steps {
        sh 'docker build -t java-imagen .'
        echo 'Build Image succes'
      }
    }

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
