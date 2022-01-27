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
        stage ('Zip artifact') {
            steps {
                sh 'mkdir petclinic'
                sh 'cp -r target/*.jar spring-petclinic'
                script{
                  zip zipFile: 'petclinic.zip', archive: false, dir: 'petclinic'  
                }
            }
        }
        stage('Publish Artifact'){
            steps{
                archiveArtifacts artifacts: 'petclinic.zip', fingerprint: true
            }
        }
    }
}

