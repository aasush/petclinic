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
			stage('artifact nexus uploader')
			steps {
				nexusArtifactUploader artifacts: [[artifactId: 'spring-petclinic',
			        classifier: '', file: 'target/petclinic.war', type: 'war']],
				credentialsId: '4f228bd9-8eca-456f-b65b-69e6efeeca7c',
				groupId: 'org.springframework.samples',
				nexusUrl: '18.224.199.231:8081/nexus',
				nexusVersion: 'nexus2',
				protocol: 'http',
				repository: 'releases',
				version: "4.2.${BUILD_NUMBER}"
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
