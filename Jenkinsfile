pipeline{
    agent any
    environment {
		DOCKERHUB_CREDENTIALS=credentials('docker-token-key')
        DOCKERUSER="charityngenge"
	    AWS_ACCESS_KEY_ID=credentials('aws-access-id')
        AWS_SECRET_ACCESS_KEY=credentials('aws-secret-id')
        AWS_DEFAULT_REGION=('us-east-1')	
	}
    stages{
        stage('Maven Build'){
            steps{
                sh "mvn clean package"
            } 
        }
		stage('Docker Build Petclinic') {

			steps {
				sh 'docker build -t $DOCKERUSER/spring-petclinic:${BUILD_NUMBER}-dev .'
			}
		}
		stage('Login to Docker HUB') {

			steps {
				sh 'echo $DOCKERHUB_CREDENTIALS_PSW | docker login -u $DOCKERHUB_CREDENTIALS_USR --password-stdin'
			}
		}

		stage('Push') {

			steps {
				sh 'docker push  $DOCKERUSER/spring-petclinic:${BUILD_NUMBER}-dev'
			}
		}
		stage('cloudformation') {
			steps{
				sh"aws cloudformation create-stack --stack-name spring-petclinic-${BUILD_NUMBER} --template-body file://infrastructure.yaml --region 'us-east-1' --parameters ParameterKey=KeyName,ParameterValue=cloudformation ParameterKey=ServerName,ParameterValue=spring-petclinic-${BUILD_NUMBER}"
			}
		}
		stage('WaitingForInstanceToComeUp') {
			steps{
				sh ' sleep 2m'
			}
		}
		stage('GetInstanceIP') {
			steps{
                sh  "aws ec2 describe-instances --filters Name=tag:Name,Values='spring-petclinic-${BUILD_NUMBER}' --query 'Reservations[].Instances[].PublicIpAddress' --output text"
			}
		}
        stage('Cleanup') {
            steps{
                sh "docker rmi $DOCKERUSER/spring-petclinic:${BUILD_NUMBER}-dev"
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
