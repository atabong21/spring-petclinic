pipeline {
  agent any
  tools {
    maven 'maven' 
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
