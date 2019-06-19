pipeline {
    agent {
    }
    options {
        skipStagesAfterUnstable()
    }
    stages {
        stage('Build') {
         steps {
			withDockerRegistry(credentialsId: 'docker-reg-credentails', url: 'https://index.docker.io/v1/') {
				image = docker.image('index.docker.io/v1//ubuntu-16:1')
				image.pull()
				docker.image('registryhub:8085/maven:3-alpine').inside {   
					sh 'mvn -B -DskipTests clean package'
				}
			}
		 }
		}
        stage('Test') {
            steps {
                sh 'mvn test'
            }
            post {
                always {
                    junit 'target/surefire-reports/*.xml'
                }
            }
        }
        stage('Deliver') { 
            steps {
                sh './jenkins/scripts/deliver.sh' 
            }
        }
    }
}