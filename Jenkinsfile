pipeline {
	agent any
	
	environment {
		DOCKERHUB_CRED=credentials('devops')
		IMAGE_NAME="vishwasmagar10/devops_maven"
	}
	
	stages {
		stage('checkout') {
			steps {
				git url:'https://github.com/vishwasmagar10/devops_maven', branch:'main'
			}
		}

		stage('Build Maven Project') {
			steps {
				sh "mvn clean package -DskipTests"
			}
		}
		
		stage('Build Docker Image') {
            		steps {
                		script {
                    			dockerImage = docker.build("${IMAGE_NAME}:latest")
                		}
            		}
        	}

		stage('Push Docker Image'){
			steps {
				script {
					docker.withRegistry('https://index.docker.io/v1/','devops'){
						dockerImage.push()
					}
				}
			}
		}	
	}
	
	post {
		success {
			echo "Pipeline Successful"
		}
		failure {
			echo "Pipeline Failure"
		}
		always {
			echo "Cleaning workspace"
			deleteDir()
		}
	}

}
