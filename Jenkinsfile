pipeline {
	agent any 

		stages {
			stage('checkout') {
				steps {
					git "https://github.com/aasush/petclinic.git"
				}
			}
			stage('build') {
				steps {
					sh 'mvn clean package'
				}
			}
			stage('archival') {
				steps {
					archiveArtifacts 'target/*.war'
				}
			}
			stage('junit test cases') {
				steps {
					junit 'target/surefire-reports/*.xml'
				}
			}
		}
post {
		always {
			notify('started')
		}
		failure {
			notify('err')
		}
		success {
			notify('success')
		}
	}
}


def notify(status) {
			emailext (
				to: 'aasushdisney@gmail.com',
			    subject: "${status}: JOB:'${env.JOB_NAME} job ${env.JOB_ID}: ${env.JENKINS_HOME}: ${env.BUILD_ID}: --${status}'",
				body: "Please go to ${BUILD_URL} and verify the build"
			)
		}
