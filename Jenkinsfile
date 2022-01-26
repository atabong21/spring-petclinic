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
        stage ('push artifact') {
            steps {
                zip zipFile: 'spring-petclinic.zip', followSymlinks: false, dir: 'target/*.jar'
                archiveArtifacts artifacts: 'target/*.jar', fingerprint: true
            }
        }
    }
}

