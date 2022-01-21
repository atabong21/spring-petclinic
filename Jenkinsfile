pipeline {
  agent any
  tools {
    maven 'maven-3.6.3' 
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
