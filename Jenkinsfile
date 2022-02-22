pipeline{
    agent any
    tools {
      maven 'Maven'
    }
    environment {
		DOCKERHUB_CREDENTIALS=credentials('dockerhubtoken')
        AWS_ACCESS_KEY_ID   = credentials('	aws_id_key')
        AWS_SECRET_ACCESS_KEY = credentials('aws_secret_key')
        AWS_DEFAULT_REGION = ('us-east-1')
	}
    stages{

        stage('Maven Build'){
            steps{
                sh "mvn clean package"
            }
            
        }

		stage('Build') {

			steps {
				sh 'docker build -t charityngenge/spring-petclinic:${BUILD_NUMBER}-dev .'
			}
		}

		stage('Login') {

			steps {

				sh 'echo $DOCKERHUB_CREDENTIALS_PSW | docker login -u $DOCKERHUB_CREDENTIALS_USR -p $DOCKERHUB_CREDENTIALS_PSW'
			}
		}

		stage('Push') {

			steps {
				sh 'docker push  charityngenge/spring-petclinic:${BUILD_NUMBER}-dev'
			}
		}
        stage('Cloudformation') {
            steps {
                sh "aws cloudformation create-stack --stack-name petclinic-${BUILD_NUMBER} --template-body file://Infrastructure/infrastructure.yaml  --region 'us-east-1' --parameters ParameterKey=KeyName,ParameterValue=petclinic"
            }
        }
        stage('CleanWorkSpace'){
            steps {
                cleanWs()
            }
        }
        
	}

	post {
		always {
			sh 'docker logout'
		}
	}
}    