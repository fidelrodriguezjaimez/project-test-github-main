node {
    def workspace = pwd()
    def TOMCAT_HOME = "/home/vagrant/jenkins-training/tomcat"
    stage('Application_Build') {
        checkout scm
        sh 'mvn clean package -DskipTests'
    }    
    stage('Application_Unit_Test') {        
        sh 'mvn -Dfilename=testng-unit.xml surefire:test'
        step([$class: 'Publisher'])
    }    
    stage('Application_Code_Analysis') {        
        withSonarQubeEnv {
            sh 'mvn sonar:sonar'
        }
    }
    stage('Application_Deploy') {
        sh "cp ${workspace}/target/spring-project.war ${TOMCAT_HOME}/webapps/"
    }    
    stage('Application_Functional_Testing') {
	sleep(time:60,unit:"SECONDS")
	sh 'mvn -Dfilename=testng-functional.xml surefire:test'
    }
}
