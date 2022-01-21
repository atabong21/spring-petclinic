pipeline {
  agent any
  tools {
    maven 'maven-3.8.4' 
  }
  stages {
    stage ('Build') {
      steps {
        dir ("/var/lib/jenkins/workspace/AndrewBanin/spring-petclinic/")
        sh './mvnw package'
      }
    }
  }
}
