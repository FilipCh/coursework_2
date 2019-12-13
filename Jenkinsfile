pipeline {
    environment{
	registry = "filipch/devopscoursework_2"

	}
   agent any

    stages {
	stage('Checkout SCM'){
            
            steps {
                checkout([$class: 'GitSCM',
				branches: [[name: '*/master']],
				doGenerateSubmoduleConfigurations: false,
				extensions: [], 
				submoduleCfg: [], 
				userRemoteConfigs: [[url: 'https://github.com/FilipCh/coursework_2.git']]])            }
        }
        
	
        stage('SonarQube Testing') {
            environment {
                scanner = tool 'SonarQubeScanner'
            }
            
            steps {
                withSonarQubeEnv('sonarqube') {
                    sh "${scanner}/bin/sonar-scanner -D sonar.login=admin -D sonar.password=admin"
                }
				 
            }
        }
       
        
        
        stage('Build Docker Image And Push To Dockerhub') {
            steps {
                script {
                    def app = docker.build("filipch/coursework_2")
                   docker.withRegistry('', "docker-hub-credentials") {
                   app.push()
                    app.push("latest")
                }
            }
          }
        }
    stage("Deploying to Kubernetes") {
            steps {
                script {
                    sshPublisher (
                        continueOnError: false, 
                        failOnError: true,
                        publishers: [
                            sshPublisherDesc(
                                configName: "production_server",
                                verbose: true,
                                transfers: [
                                    sshTransfer(
                                        execCommand: "kubectl set image deployment/coursework_2 coursework_2=filipch/coursework_2:latest"
                                    )
                                ]
                            )
                        ]
                    )
                }
            }
        }
	
	}
	}