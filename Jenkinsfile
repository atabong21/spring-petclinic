pipeline{
    agent any
    tools {
      maven 'Maven'
    }
    stages {    
        stage('Maven Build') {
            steps{
                sh "mvn clean package"
            }
        }
        stage('Zip and Publish artifacts') {
            steps {
                 zip zipFile: 'spring-petclinic.zip' , archive: false, dir: 'target/*.jar'
                 archiveArtifacts artifacts: 'spring-petclinic.zip' , fingerprint: true
            }
        }
    }
}